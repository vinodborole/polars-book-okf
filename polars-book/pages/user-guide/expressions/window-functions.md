---
type: Web Page
title: Window functions - Polars user guide
resource: https://docs.pola.rs/user-guide/expressions/window-functions
timestamp: '2026-08-03T09:49:29.273788+00:00'
---

# Window functions

Window functions are expressions with superpowers. They allow you to perform aggregations on groups
within the context `select`. Let's get a feel for what that means.

First, we load a Pokémon dataset:

```
import polars as pl
types = (
    "Grass Water Fire Normal Ground Electric Psychic Fighting Bug Steel "
    "Flying Dragon Dark Ghost Poison Rock Ice Fairy".split()
)
type_enum = pl.Enum(types)
# then let's load some csv data with information about pokemon
pokemon = pl.read_csv(
    "docs/assets/data/pokemon.csv",
).cast({"Type 1": type_enum, "Type 2": type_enum})
print(pokemon.head())
```
  [`CsvReader`](https://docs.pola.rs/api/rust/dev/polars/prelude/struct.CsvReader.html) ·  [Available on feature csv](/user-guide/installation/#feature-flags)

```
use polars::prelude::*;
use reqwest::blocking::Client;
let data: Vec<u8> = Client::new()
    .get("https://gist.githubusercontent.com/ritchie46/cac6b337ea52281aa23c049250a4ff03/raw/89a957ff3919d90e6ef2d34235e6bf22304f3366/pokemon.csv")
    .send()?
    .text()?
    .bytes()
    .collect();
let file = std::io::Cursor::new(data);
let df = CsvReadOptions::default()
    .with_has_header(true)
    .into_reader_with_file_handle(file)
    .finish()?;
println!("{}", df.head(Some(5)));
```
```
shape: (5, 13)
┌─────┬───────────────────────┬────────┬────────┬───┬─────────┬───────┬────────────┬───────────┐
│ #   ┆ Name                  ┆ Type 1 ┆ Type 2 ┆ … ┆ Sp. Def ┆ Speed ┆ Generation ┆ Legendary │
│ --- ┆ ---                   ┆ ---    ┆ ---    ┆   ┆ ---     ┆ ---   ┆ ---        ┆ ---       │
│ i64 ┆ str                   ┆ enum   ┆ enum   ┆   ┆ i64     ┆ i64   ┆ i64        ┆ bool      │
╞═════╪═══════════════════════╪════════╪════════╪═══╪═════════╪═══════╪════════════╪═══════════╡
│ 1   ┆ Bulbasaur             ┆ Grass  ┆ Poison ┆ … ┆ 65      ┆ 45    ┆ 1          ┆ false     │
│ 2   ┆ Ivysaur               ┆ Grass  ┆ Poison ┆ … ┆ 80      ┆ 60    ┆ 1          ┆ false     │
│ 3   ┆ Venusaur              ┆ Grass  ┆ Poison ┆ … ┆ 100     ┆ 80    ┆ 1          ┆ false     │
│ 3   ┆ VenusaurMega Venusaur ┆ Grass  ┆ Poison ┆ … ┆ 120     ┆ 80    ┆ 1          ┆ false     │
│ 4   ┆ Charmander            ┆ Fire   ┆ null   ┆ … ┆ 50      ┆ 65    ┆ 1          ┆ false     │
└─────┴───────────────────────┴────────┴────────┴───┴─────────┴───────┴────────────┴───────────┘
```
## Operations per group

Window functions are ideal when we want to perform an operation within a group. For instance,
suppose we want to rank our Pokémon by the column “Speed”. However, instead of a global ranking, we
want to rank the speed within each group defined by the column “Type 1”. We write the expression to
rank the data by the column “Speed” and then we add the function `over` to specify that this should
happen over the unique values of the column “Type 1”:

```
result = pokemon.select(
    pl.col("Name", "Type 1"),
    pl.col("Speed").rank("dense", descending=True).over("Type 1").alias("Speed rank"),
)
print(result)
```
```
let result = df
    .clone()
    .lazy()
    .select([
        col("Name"),
        col("Type 1"),
        col("Speed")
            .rank(
                RankOptions {
                    method: RankMethod::Dense,
                    descending: true,
                },
                None,
            )
            .over(["Type 1"])?
            .alias("Speed rank"),
    ])
    .collect()?;
println!("{result}");
```
```
shape: (163, 3)
┌───────────────────────┬─────────┬────────────┐
│ Name                  ┆ Type 1  ┆ Speed rank │
│ ---                   ┆ ---     ┆ ---        │
│ str                   ┆ enum    ┆ u32        │
╞═══════════════════════╪═════════╪════════════╡
│ Bulbasaur             ┆ Grass   ┆ 6          │
│ Ivysaur               ┆ Grass   ┆ 3          │
│ Venusaur              ┆ Grass   ┆ 1          │
│ VenusaurMega Venusaur ┆ Grass   ┆ 1          │
│ Charmander            ┆ Fire    ┆ 7          │
│ …                     ┆ …       ┆ …          │
│ Moltres               ┆ Fire    ┆ 5          │
│ Dratini               ┆ Dragon  ┆ 3          │
│ Dragonair             ┆ Dragon  ┆ 2          │
│ Dragonite             ┆ Dragon  ┆ 1          │
│ Mewtwo                ┆ Psychic ┆ 2          │
└───────────────────────┴─────────┴────────────┘
```
To help visualise this operation, you may imagine that Polars selects the subsets of the data that share the same value for the column “Type 1” and then computes the ranking expression only for those values. Then, the results for that specific group are projected back to the original rows and Polars does this for all of the existing groups. The diagram below highlights the ranking computation for the Pokémon with “Type 1” equal to “Grass”.

Note how the row for the Pokémon “Golbat” has a “Speed” value of `90`, which is greater than the
value `80` of the Pokémon “Venusaur”, and yet the latter was ranked 1 because “Golbat” and “Venusar”
do not share the same value for the column “Type 1”.

The function `over` accepts an arbitrary number of expressions to specify the groups over which to
perform the computations. We can repeat the ranking above, but over the combination of the columns
“Type 1” and “Type 2” for a more fine-grained ranking:

```
shape: (163, 4)
┌───────────────────────┬─────────┬────────┬────────────┐
│ Name                  ┆ Type 1  ┆ Type 2 ┆ Speed rank │
│ ---                   ┆ ---     ┆ ---    ┆ ---        │
│ str                   ┆ enum    ┆ enum   ┆ u32        │
╞═══════════════════════╪═════════╪════════╪════════════╡
│ Bulbasaur             ┆ Grass   ┆ Poison ┆ 6          │
│ Ivysaur               ┆ Grass   ┆ Poison ┆ 3          │
│ Venusaur              ┆ Grass   ┆ Poison ┆ 1          │
│ VenusaurMega Venusaur ┆ Grass   ┆ Poison ┆ 1          │
│ Charmander            ┆ Fire    ┆ null   ┆ 7          │
│ …                     ┆ …       ┆ …      ┆ …          │
│ Moltres               ┆ Fire    ┆ Flying ┆ 2          │
│ Dratini               ┆ Dragon  ┆ null   ┆ 2          │
│ Dragonair             ┆ Dragon  ┆ null   ┆ 1          │
│ Dragonite             ┆ Dragon  ┆ Flying ┆ 1          │
│ Mewtwo                ┆ Psychic ┆ null   ┆ 2          │
└───────────────────────┴─────────┴────────┴────────────┘
```
In general, the results you get with the function `over` can also be achieved with
[an aggregation](../aggregation/) followed by a call to the function `explode`, although the rows
would be in a different order:

```
result = (
    pokemon.group_by("Type 1")
    .agg(
        pl.col("Name"),
        pl.col("Speed").rank("dense", descending=True).alias("Speed rank"),
    )
    .select(pl.col("Name"), pl.col("Type 1"), pl.col("Speed rank"))
    .explode("Name", "Speed rank", empty_as_null=False)
)
print(result)
```
```
// Contribute the Rust translation of the Python example by opening a PR.
```
```
shape: (163, 3)
┌───────────────────────┬────────┬────────────┐
│ Name                  ┆ Type 1 ┆ Speed rank │
│ ---                   ┆ ---    ┆ ---        │
│ str                   ┆ enum   ┆ u32        │
╞═══════════════════════╪════════╪════════════╡
│ Sandshrew             ┆ Ground ┆ 5          │
│ Sandslash             ┆ Ground ┆ 3          │
│ Diglett               ┆ Ground ┆ 2          │
│ Dugtrio               ┆ Ground ┆ 1          │
│ Cubone                ┆ Ground ┆ 6          │
│ …                     ┆ …      ┆ …          │
│ GyaradosMega Gyarados ┆ Water  ┆ 5          │
│ Lapras                ┆ Water  ┆ 13         │
│ Vaporeon              ┆ Water  ┆ 11         │
│ Clefairy              ┆ Fairy  ┆ 2          │
│ Clefable              ┆ Fairy  ┆ 1          │
└───────────────────────┴────────┴────────────┘
```
This shows that, usually, `group_by` and `over` produce results of different shapes:

- `group_by` usually produces a resulting dataframe with as many rows as groups used for
  aggregating; and
- `over` usually produces a dataframe with the same number of rows as the original.

The function `over` does not always produce results with the same number of rows as the original
dataframe, and that is what we explore next.

## Mapping results to dataframe rows

The function `over` accepts a parameter `mapping_strategy` that determines how the results of the
expression over the group are mapped back to the rows of the dataframe.

### `group_to_rows`

The default behaviour is `"group_to_rows"`: the result of the expression over the group should be
the same length as the group and the results are mapped back to the rows of that group.

If the order of the rows is not relevant, the option `"explode"` is more performant. Instead of
mapping the resulting values to the original rows, Polars creates a new dataframe where values from
the same group are next to each other. To help understand the distinction, consider the following
dataframe:

```
shape: (6, 3)
┌─────────┬─────────┬──────┐
│ athlete ┆ country ┆ rank │
│ ---     ┆ ---     ┆ ---  │
│ str     ┆ str     ┆ i64  │
╞═════════╪═════════╪══════╡
│ A       ┆ PT      ┆ 6    │
│ B       ┆ NL      ┆ 1    │
│ C       ┆ NL      ┆ 5    │
│ D       ┆ PT      ┆ 4    │
│ E       ┆ PT      ┆ 2    │
│ F       ┆ NL      ┆ 3    │
└─────────┴─────────┴──────┘
```
We can sort the athletes by rank within their own countries. If we do so, the Dutch athletes were in the second, third, and sixth, rows, and they will remain there. What will change is the order of the names of the athletes, which goes from “B”, “C”, and “F”, to “B”, “F”, and “C”:

```
shape: (6, 3)
┌─────────┬──────┬─────────┐
│ athlete ┆ rank ┆ country │
│ ---     ┆ ---  ┆ ---     │
│ str     ┆ i64  ┆ str     │
╞═════════╪══════╪═════════╡
│ E       ┆ 2    ┆ PT      │
│ B       ┆ 1    ┆ NL      │
│ F       ┆ 3    ┆ NL      │
│ D       ┆ 4    ┆ PT      │
│ A       ┆ 6    ┆ PT      │
│ C       ┆ 5    ┆ NL      │
└─────────┴──────┴─────────┘
```
The diagram below represents this transformation:

### `explode`

If we set the parameter `mapping_strategy` to `"explode"`, then athletes of the same country are
grouped together, but the final order of the rows – with respect to the countries – will not be the
same, as the diagram shows:

Because Polars does not need to keep track of the positions of the rows of each group, using
`"explode"` is typically faster than `"group_to_rows"`. However, using `"explode"` also requires
more care because it implies reordering the other columns that we wish to keep. The code that
produces this result follows

```
shape: (6, 3)
┌─────────┬─────────┬──────┐
│ athlete ┆ country ┆ rank │
│ ---     ┆ ---     ┆ ---  │
│ str     ┆ str     ┆ i64  │
╞═════════╪═════════╪══════╡
│ E       ┆ PT      ┆ 2    │
│ D       ┆ PT      ┆ 4    │
│ A       ┆ PT      ┆ 6    │
│ B       ┆ NL      ┆ 1    │
│ F       ┆ NL      ┆ 3    │
│ C       ┆ NL      ┆ 5    │
└─────────┴─────────┴──────┘
```
### `join`

Another possible value for the parameter `mapping_strategy` is `"join"`, which aggregates the
resulting values in a list and repeats the list over all rows of the same group:

```
shape: (6, 3)
┌─────────┬─────────┬───────────┐
│ athlete ┆ country ┆ rank      │
│ ---     ┆ ---     ┆ ---       │
│ str     ┆ str     ┆ list[i64] │
╞═════════╪═════════╪═══════════╡
│ A       ┆ PT      ┆ [2, 4, 6] │
│ B       ┆ NL      ┆ [1, 3, 5] │
│ C       ┆ NL      ┆ [1, 3, 5] │
│ D       ┆ PT      ┆ [2, 4, 6] │
│ E       ┆ PT      ┆ [2, 4, 6] │
│ F       ┆ NL      ┆ [1, 3, 5] │
└─────────┴─────────┴───────────┘
```
## Windowed aggregation expressions

In case the expression applied to the values of a group produces a scalar value, the scalar is broadcast across the rows of the group:

```
result = pokemon.select(
    pl.col("Name", "Type 1", "Speed"),
    pl.col("Speed").mean().over(pl.col("Type 1")).alias("Mean speed in group"),
)
print(result)
```
```
let result = df
    .clone()
    .lazy()
    .select([
        col("Name"),
        col("Type 1"),
        col("Speed"),
        col("Speed")
            .mean()
            .over(["Type 1"])?
            .alias("Mean speed in group"),
    ])
    .collect()?;
println!("{result}");
```
```
shape: (163, 4)
┌───────────────────────┬─────────┬───────┬─────────────────────┐
│ Name                  ┆ Type 1  ┆ Speed ┆ Mean speed in group │
│ ---                   ┆ ---     ┆ ---   ┆ ---                 │
│ str                   ┆ enum    ┆ i64   ┆ f64                 │
╞═══════════════════════╪═════════╪═══════╪═════════════════════╡
│ Bulbasaur             ┆ Grass   ┆ 45    ┆ 54.230769           │
│ Ivysaur               ┆ Grass   ┆ 60    ┆ 54.230769           │
│ Venusaur              ┆ Grass   ┆ 80    ┆ 54.230769           │
│ VenusaurMega Venusaur ┆ Grass   ┆ 80    ┆ 54.230769           │
│ Charmander            ┆ Fire    ┆ 65    ┆ 86.285714           │
│ …                     ┆ …       ┆ …     ┆ …                   │
│ Moltres               ┆ Fire    ┆ 90    ┆ 86.285714           │
│ Dratini               ┆ Dragon  ┆ 50    ┆ 66.666667           │
│ Dragonair             ┆ Dragon  ┆ 70    ┆ 66.666667           │
│ Dragonite             ┆ Dragon  ┆ 80    ┆ 66.666667           │
│ Mewtwo                ┆ Psychic ┆ 130   ┆ 99.25               │
└───────────────────────┴─────────┴───────┴─────────────────────┘
```
## More examples

For more exercises, below are some window functions for us to compute:

- sort all Pokémon by type;
- select the first `3` Pokémon per type as`"Type 1"` ;
- sort the Pokémon within a type by speed in descending order and select the first `3` as`"fastest/group"` ;
- sort the Pokémon within a type by attack in descending order and select the first `3` as`"strongest/group"` ; and
- sort the Pokémon within a type by name and select the first `3` as`"sorted_by_alphabet"` .

```
result = pokemon.sort("Type 1").select(
    pl.col("Type 1").head(3).over("Type 1", mapping_strategy="explode"),
    pl.col("Name")
    .sort_by(pl.col("Speed"), descending=True)
    .head(3)
    .over("Type 1", mapping_strategy="explode")
    .alias("fastest/group"),
    pl.col("Name")
    .sort_by(pl.col("Attack"), descending=True)
    .head(3)
    .over("Type 1", mapping_strategy="explode")
    .alias("strongest/group"),
    pl.col("Name")
    .sort()
    .head(3)
    .over("Type 1", mapping_strategy="explode")
    .alias("sorted_by_alphabet"),
)
print(result)
```
```
let result = df
    .lazy()
    .select([
        col("Type 1")
            .head(Some(3))
            .over_with_options(Some(["Type 1"]), None, WindowMapping::Explode)?
            .explode(ExplodeOptions {
                empty_as_null: false,
                keep_nulls: false,
            }),
        col("Name")
            .sort_by(
                ["Speed"],
                SortMultipleOptions::default().with_order_descending(true),
            )
            .head(Some(3))
            .over_with_options(Some(["Type 1"]), None, WindowMapping::Explode)?
            .explode(ExplodeOptions {
                empty_as_null: false,
                keep_nulls: false,
            })
            .alias("fastest/group"),
        col("Name")
            .sort_by(
                ["Attack"],
                SortMultipleOptions::default().with_order_descending(true),
            )
            .head(Some(3))
            .over_with_options(Some(["Type 1"]), None, WindowMapping::Explode)?
            .explode(ExplodeOptions {
                empty_as_null: false,
                keep_nulls: false,
            })
            .alias("strongest/group"),
        col("Name")
            .sort(Default::default())
            .head(Some(3))
            .over_with_options(Some(["Type 1"]), None, WindowMapping::Explode)?
            .explode(ExplodeOptions {
                empty_as_null: false,
                keep_nulls: false,
            })
            .alias("sorted_by_alphabet"),
    ])
    .collect()?;
println!("{result:?}");
```
```
shape: (43, 4)
┌────────┬───────────────────────┬───────────────────────┬─────────────────────────┐
│ Type 1 ┆ fastest/group         ┆ strongest/group       ┆ sorted_by_alphabet      │
│ ---    ┆ ---                   ┆ ---                   ┆ ---                     │
│ enum   ┆ str                   ┆ str                   ┆ str                     │
╞════════╪═══════════════════════╪═══════════════════════╪═════════════════════════╡
│ Grass  ┆ Venusaur              ┆ Victreebel            ┆ Bellsprout              │
│ Grass  ┆ VenusaurMega Venusaur ┆ VenusaurMega Venusaur ┆ Bulbasaur               │
│ Grass  ┆ Victreebel            ┆ Exeggutor             ┆ Exeggcute               │
│ Water  ┆ Starmie               ┆ GyaradosMega Gyarados ┆ Blastoise               │
│ Water  ┆ Tentacruel            ┆ Kingler               ┆ BlastoiseMega Blastoise │
│ …      ┆ …                     ┆ …                     ┆ …                       │
│ Rock   ┆ Kabutops              ┆ Kabutops              ┆ Geodude                 │
│ Ice    ┆ Jynx                  ┆ Articuno              ┆ Articuno                │
│ Ice    ┆ Articuno              ┆ Jynx                  ┆ Jynx                    │
│ Fairy  ┆ Clefable              ┆ Clefable              ┆ Clefable                │
│ Fairy  ┆ Clefairy              ┆ Clefairy              ┆ Clefairy                │
└────────┴───────────────────────┴───────────────────────┴─────────────────────────┘
```

# Citations

1. Source page: https://docs.pola.rs/user-guide/expressions/window-functions
