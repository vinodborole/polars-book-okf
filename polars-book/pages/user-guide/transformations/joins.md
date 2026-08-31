---
type: Web Page
title: Joins - Polars user guide
resource: https://docs.pola.rs/user-guide/transformations/joins
timestamp: '2026-08-31T12:59:29.541052+00:00'
---

# Joins

A join operation combines columns from one or more dataframes into a new dataframe. The different “joining strategies” and matching criteria used by the different types of joins influence how columns are combined and also what rows are included in the result of the join operation.

The most common type of join is an “equi join”, in which rows are matched by a key expression. Polars supports several joining strategies for equi joins, which determine exactly how we handle the matching of rows. Polars also supports “non-equi joins”, a type of join where the matching criterion is not an equality, and a type of join where rows are matched by key proximity, called “asof join”.

## Quick reference table

The table below acts as a quick reference for people who know what they are looking for. If you want to learn about joins in general and how to work with them in Polars, feel free to skip the table and keep reading below.

 [`join`](https://docs.pola.rs/api/rust/dev/polars/prelude/trait.DataFrameJoinOps.html#method.join)
( [semi_anti_join](/user-guide/installation/#feature-flags) needed for some options.)
 [`join_asof_by`](https://docs.pola.rs/api/rust/dev/polars/prelude/trait.AsofJoinBy.html#method.join_asof_by)
 [Available on feature asof_join](/user-guide/installation/#feature-flags)
 [`join_where`](https://docs.rs/polars/latest/polars/prelude/struct.JoinBuilder.html#method.join_where)
 [Available on feature iejoin](/user-guide/installation/#feature-flags)

| Type | Function | Brief description | 
|---|---|---|
| Equi inner join | `join(..., how="inner")` | Keeps rows that matched both on the left and right. | 
| Equi left outer join | `join(..., how="left")` | Keeps all rows from the left plus matching rows from the right. Non-matching rows from the left have their right columns filled with `null` . | 
| Equi right outer join | `join(..., how="right")` | Keeps all rows from the right plus matching rows from the left. Non-matching rows from the right have their left columns filled with `null` . | 
| Equi full join | `join(..., how="full")` | Keeps all rows from either dataframe, regardless of whether they match or not. Non-matching rows from one side have the columns from the other side filled with `null` . | 
| Equi semi join | `join(..., how="semi")` | Keeps rows from the left that have a match on the right. | 
| Equi anti join | `join(..., how="anti")` | Keeps rows from the left that do not have a match on the right. | 
| Non-equi inner join | `join_where` | Finds all possible pairings of rows from the left and right that satisfy the given predicate(s). | 
| Asof join | `join_asof` /`join_asof_by` | Like a left outer join, but matches on the nearest key instead of on exact key matches. | 
| Cartesian product | `join(..., how="cross")` | Computes the [Cartesian product](https://en.wikipedia.org/wiki/Cartesian_product) of the two dataframes. | 

## Equi joins

In an equi join, rows are matched by checking equality of a key expression. You can do an equi join
with the function `join` by specifying the name of the column to be used as key. For the examples,
we will be loading some (modified) Monopoly property data.

First, we load a dataframe that contains property names and their colour group in the game:

```
import polars as pl
props_groups = pl.read_csv("docs/assets/data/monopoly_props_groups.csv").head(5)
print(props_groups)
```
```
let props_groups = CsvReadOptions::default()
    .with_has_header(true)
    .try_into_reader_with_file_path(Some("docs/assets/data/monopoly_props_groups.csv".into()))?
    .finish()?
    .head(Some(5));
println!("{props_groups}");
```
```
shape: (5, 2)
┌──────────────────────┬────────────┐
│ property_name        ┆ group      │
│ ---                  ┆ ---        │
│ str                  ┆ str        │
╞══════════════════════╪════════════╡
│ Old Ken Road         ┆ brown      │
│ Whitechapel Road     ┆ brown      │
│ The Shire            ┆ fantasy    │
│ Kings Cross Station  ┆ stations   │
│ The Angel, Islington ┆ light_blue │
└──────────────────────┴────────────┘
```
Next, we load a dataframe that contains property names and their price in the game:

```
props_prices = pl.read_csv("docs/assets/data/monopoly_props_prices.csv").head(5)
print(props_prices)
```
```
let props_prices = CsvReadOptions::default()
    .with_has_header(true)
    .try_into_reader_with_file_path(Some("docs/assets/data/monopoly_props_prices.csv".into()))?
    .finish()?
    .head(Some(5));
println!("{props_prices}");
```
```
shape: (5, 2)
┌──────────────────────┬──────┐
│ property_name        ┆ cost │
│ ---                  ┆ ---  │
│ str                  ┆ i64  │
╞══════════════════════╪══════╡
│ Old Ken Road         ┆ 60   │
│ Whitechapel Road     ┆ 60   │
│ Sesame Street        ┆ 100  │
│ Kings Cross Station  ┆ 200  │
│ The Angel, Islington ┆ 100  │
└──────────────────────┴──────┘
```
Now, we join both dataframes to create a dataframe that contains property names, colour groups, and prices:

```
result = props_groups.join(props_prices, on="property_name")
print(result)
```
```
// In Rust, we cannot use the shorthand of specifying a common
// column name just once.
let result = props_groups
    .clone()
    .lazy()
    .join(
        props_prices.clone().lazy(),
        [col("property_name")],
        [col("property_name")],
        JoinArgs::default(),
    )?
    .collect()?;
println!("{result}");
```
```
shape: (4, 3)
┌──────────────────────┬────────────┬──────┐
│ property_name        ┆ group      ┆ cost │
│ ---                  ┆ ---        ┆ ---  │
│ str                  ┆ str        ┆ i64  │
╞══════════════════════╪════════════╪══════╡
│ Old Ken Road         ┆ brown      ┆ 60   │
│ Whitechapel Road     ┆ brown      ┆ 60   │
│ Kings Cross Station  ┆ stations   ┆ 200  │
│ The Angel, Islington ┆ light_blue ┆ 100  │
└──────────────────────┴────────────┴──────┘
```
The result has four rows but both dataframes used in the operation had five rows. Polars uses a
joining strategy to determine what happens with rows that have multiple matches or with rows that
have no match at all. By default, Polars computes an “inner join” but there are
[other join strategies that we show next](#join-strategies).

In the example above, the two dataframes conveniently had the column we wish to use as key with the same name and with the values in the exact same format. Suppose, for the sake of argument, that one of the dataframes had a differently named column and the other had the property names in lower case:

```
props_groups2 = props_groups.with_columns(
    pl.col("property_name").str.to_lowercase(),
)
print(props_groups2)
```
  [`str namespace`](https://docs.pola.rs/api/rust/dev/polars/prelude/trait.StringNameSpaceImpl.html) ·  [Available on feature strings](/user-guide/installation/#feature-flags)

```
let props_groups2 = props_groups
    .clone()
    .lazy()
    .with_column(col("property_name").str().to_lowercase())
    .collect()?;
println!("{props_groups2}");
```
```
shape: (5, 2)
┌──────────────────────┬────────────┐
│ property_name        ┆ group      │
│ ---                  ┆ ---        │
│ str                  ┆ str        │
╞══════════════════════╪════════════╡
│ old ken road         ┆ brown      │
│ whitechapel road     ┆ brown      │
│ the shire            ┆ fantasy    │
│ kings cross station  ┆ stations   │
│ the angel, islington ┆ light_blue │
└──────────────────────┴────────────┘
```
```
props_prices2 = props_prices.select(
    pl.col("property_name").alias("name"), pl.col("cost")
)
print(props_prices2)
```
```
let props_prices2 = props_prices
    .clone()
    .lazy()
    .select([col("property_name").alias("name"), col("cost")])
    .collect()?;
println!("{props_prices2}");
```
```
shape: (5, 2)
┌──────────────────────┬──────┐
│ name                 ┆ cost │
│ ---                  ┆ ---  │
│ str                  ┆ i64  │
╞══════════════════════╪══════╡
│ Old Ken Road         ┆ 60   │
│ Whitechapel Road     ┆ 60   │
│ Sesame Street        ┆ 100  │
│ Kings Cross Station  ┆ 200  │
│ The Angel, Islington ┆ 100  │
└──────────────────────┴──────┘
```
In a situation like this, where we may want to perform the same join as before, we can leverage
`join`'s flexibility and specify arbitrary expressions to compute the joining key on the left and on
the right, allowing one to compute row keys dynamically:

```
result = props_groups2.join(
    props_prices2,
    left_on="property_name",
    right_on=pl.col("name").str.to_lowercase(),
)
print(result)
```
  [`join`](https://docs.pola.rs/api/rust/dev/polars/prelude/trait.DataFrameJoinOps.html#method.join) ·  [`str namespace`](https://docs.pola.rs/api/rust/dev/polars/prelude/trait.StringNameSpaceImpl.html) ·  [Available on feature strings](/user-guide/installation/#feature-flags)

```
let result = props_groups2
    .lazy()
    .join(
        props_prices2.lazy(),
        [col("property_name")],
        [col("name").str().to_lowercase()],
        JoinArgs::default(),
    )?
    .collect()?;
println!("{result}");
```
```
shape: (4, 4)
┌──────────────────────┬────────────┬──────────────────────┬──────┐
│ property_name        ┆ group      ┆ name                 ┆ cost │
│ ---                  ┆ ---        ┆ ---                  ┆ ---  │
│ str                  ┆ str        ┆ str                  ┆ i64  │
╞══════════════════════╪════════════╪══════════════════════╪══════╡
│ old ken road         ┆ brown      ┆ Old Ken Road         ┆ 60   │
│ whitechapel road     ┆ brown      ┆ Whitechapel Road     ┆ 60   │
│ kings cross station  ┆ stations   ┆ Kings Cross Station  ┆ 200  │
│ the angel, islington ┆ light_blue ┆ The Angel, Islington ┆ 100  │
└──────────────────────┴────────────┴──────────────────────┴──────┘
```
Because we are joining on the right with an expression, Polars preserves the column “property_name” from the left and the column “name” from the right so we can have access to the original values that the key expressions were applied to.

## Join strategies

When computing a join with `df1.join(df2, ...)`, we can specify one of many different join
strategies. A join strategy specifies what rows to keep from each dataframe based on whether they
match rows from the other dataframe.

### Inner join

In an inner join the resulting dataframe only contains the rows from the left and right dataframes
that matched. That is the default strategy used by `join` and above we can see an example of that.
We repeat the example here and explicitly specify the join strategy:

```
shape: (4, 3)
┌──────────────────────┬────────────┬──────┐
│ property_name        ┆ group      ┆ cost │
│ ---                  ┆ ---        ┆ ---  │
│ str                  ┆ str        ┆ i64  │
╞══════════════════════╪════════════╪══════╡
│ Old Ken Road         ┆ brown      ┆ 60   │
│ Whitechapel Road     ┆ brown      ┆ 60   │
│ Kings Cross Station  ┆ stations   ┆ 200  │
│ The Angel, Islington ┆ light_blue ┆ 100  │
└──────────────────────┴────────────┴──────┘
```
The result does not include the row from `props_groups` that contains “The Shire” and the result
also does not include the row from `props_prices` that contains “Sesame Street”.

### Left join

A left outer join is a join where the result contains all the rows from the left dataframe and the rows of the right dataframe that matched any rows from the left dataframe.

```
shape: (5, 3)
┌──────────────────────┬────────────┬──────┐
│ property_name        ┆ group      ┆ cost │
│ ---                  ┆ ---        ┆ ---  │
│ str                  ┆ str        ┆ i64  │
╞══════════════════════╪════════════╪══════╡
│ Old Ken Road         ┆ brown      ┆ 60   │
│ Whitechapel Road     ┆ brown      ┆ 60   │
│ The Shire            ┆ fantasy    ┆ null │
│ Kings Cross Station  ┆ stations   ┆ 200  │
│ The Angel, Islington ┆ light_blue ┆ 100  │
└──────────────────────┴────────────┴──────┘
```
If there are any rows from the left dataframe that have no matching rows on the right dataframe,
they get the value `null` on the new columns.

### Right join

Computationally speaking, a right outer join is exactly the same as a left outer join, but with the arguments swapped. Here is an example:

```
shape: (5, 3)
┌────────────┬──────────────────────┬──────┐
│ group      ┆ property_name        ┆ cost │
│ ---        ┆ ---                  ┆ ---  │
│ str        ┆ str                  ┆ i64  │
╞════════════╪══════════════════════╪══════╡
│ brown      ┆ Old Ken Road         ┆ 60   │
│ brown      ┆ Whitechapel Road     ┆ 60   │
│ null       ┆ Sesame Street        ┆ 100  │
│ stations   ┆ Kings Cross Station  ┆ 200  │
│ light_blue ┆ The Angel, Islington ┆ 100  │
└────────────┴──────────────────────┴──────┘
```
We show that `df1.join(df2, how="right", ...)` is the same as `df2.join(df1, how="left", ...)`, up
to the order of the columns of the result, with the computation below:

```
print(
    result.equals(
        props_prices.join(
            props_groups,
            on="property_name",
            how="left",
            # Reorder the columns to match the order from above.
        ).select(pl.col("group"), pl.col("property_name"), pl.col("cost"))
    )
)
```
```
// `equals_missing` is needed instead of `equals`
// so that missing values compare as equal.
let dfs_match = result.equals_missing(
    &props_prices
        .clone()
        .lazy()
        .join(
            props_groups.clone().lazy(),
            [col("property_name")],
            [col("property_name")],
            JoinArgs::new(JoinType::Left),
        )?
        .select([
            // Reorder the columns to match the order of `result`.
            col("group"),
            col("property_name"),
            col("cost"),
        ])
        .collect()?,
);
println!("{dfs_match}");
```
```
True
```
### Full join

A full outer join will keep all of the rows from the left and right dataframes, even if they don't have matching rows in the other dataframe:

```
shape: (6, 4)
┌──────────────────────┬────────────┬──────────────────────┬──────┐
│ property_name        ┆ group      ┆ property_name_right  ┆ cost │
│ ---                  ┆ ---        ┆ ---                  ┆ ---  │
│ str                  ┆ str        ┆ str                  ┆ i64  │
╞══════════════════════╪════════════╪══════════════════════╪══════╡
│ Old Ken Road         ┆ brown      ┆ Old Ken Road         ┆ 60   │
│ Whitechapel Road     ┆ brown      ┆ Whitechapel Road     ┆ 60   │
│ null                 ┆ null       ┆ Sesame Street        ┆ 100  │
│ Kings Cross Station  ┆ stations   ┆ Kings Cross Station  ┆ 200  │
│ The Angel, Islington ┆ light_blue ┆ The Angel, Islington ┆ 100  │
│ The Shire            ┆ fantasy    ┆ null                 ┆ null │
└──────────────────────┴────────────┴──────────────────────┴──────┘
```
In this case, we see that we get two columns `property_name` and `property_name_right` to make up
for the fact that we are matching on the column `property_name` of both dataframes and there are
some names for which there are no matches. The two columns help differentiate the source of each row
data. If we wanted to force `join` to coalesce the two columns `property_name` into a single column,
we could set `coalesce=True` explicitly:

```
result = props_groups.join(
    props_prices,
    on="property_name",
    how="full",
    coalesce=True,
)
print(result)
```
```
let result = props_groups
    .clone()
    .lazy()
    .join(
        props_prices.clone().lazy(),
        [col("property_name")],
        [col("property_name")],
        JoinArgs::new(JoinType::Full).with_coalesce(JoinCoalesce::CoalesceColumns),
    )?
    .collect()?;
println!("{result}");
```
```
shape: (6, 3)
┌──────────────────────┬────────────┬──────┐
│ property_name        ┆ group      ┆ cost │
│ ---                  ┆ ---        ┆ ---  │
│ str                  ┆ str        ┆ i64  │
╞══════════════════════╪════════════╪══════╡
│ Old Ken Road         ┆ brown      ┆ 60   │
│ Whitechapel Road     ┆ brown      ┆ 60   │
│ Sesame Street        ┆ null       ┆ 100  │
│ Kings Cross Station  ┆ stations   ┆ 200  │
│ The Angel, Islington ┆ light_blue ┆ 100  │
│ The Shire            ┆ fantasy    ┆ null │
└──────────────────────┴────────────┴──────┘
```
When not set, the parameter `coalesce` is determined automatically from the join strategy and the
key(s) specified, which is why the inner, left, and right, joins acted as if `coalesce=True`, even
though we didn't set it.

### Semi join

A semi join will return the rows of the left dataframe that have a match in the right dataframe, but we do not actually join the matching rows:

```
result = props_groups.join(props_prices, on="property_name", how="semi")
print(result)
```
  [`join`](https://docs.pola.rs/api/rust/dev/polars/prelude/trait.DataFrameJoinOps.html#method.join) ·  [Available on feature semi_anti_join](/user-guide/installation/#feature-flags)

```
let result = props_groups
    .clone()
    .lazy()
    .join(
        props_prices.clone().lazy(),
        [col("property_name")],
        [col("property_name")],
        JoinArgs::new(JoinType::Semi),
    )?
    .collect()?;
println!("{result}");
```
```
shape: (4, 2)
┌──────────────────────┬────────────┐
│ property_name        ┆ group      │
│ ---                  ┆ ---        │
│ str                  ┆ str        │
╞══════════════════════╪════════════╡
│ Old Ken Road         ┆ brown      │
│ Whitechapel Road     ┆ brown      │
│ Kings Cross Station  ┆ stations   │
│ The Angel, Islington ┆ light_blue │
└──────────────────────┴────────────┘
```
A semi join acts as a sort of row filter based on a second dataframe.

### Anti join

Conversely, an anti join will return the rows of the left dataframe that do not have a match in the right dataframe:

```
result = props_groups.join(props_prices, on="property_name", how="anti")
print(result)
```
  [`join`](https://docs.pola.rs/api/rust/dev/polars/prelude/trait.DataFrameJoinOps.html#method.join) ·  [Available on feature semi_anti_join](/user-guide/installation/#feature-flags)

```
let result = props_groups
    .lazy()
    .join(
        props_prices.clone().lazy(),
        [col("property_name")],
        [col("property_name")],
        JoinArgs::new(JoinType::Anti),
    )?
    .collect()?;
println!("{result}");
```
```
shape: (1, 2)
┌───────────────┬─────────┐
│ property_name ┆ group   │
│ ---           ┆ ---     │
│ str           ┆ str     │
╞═══════════════╪═════════╡
│ The Shire     ┆ fantasy │
└───────────────┴─────────┘
```
## Non-equi joins

In a non-equi join matches between the left and right dataframes are computed differently. Instead of looking for matches on key expressions, we provide a single predicate that determines what rows of the left dataframe can be paired up with what rows of the right dataframe.

For example, consider the following Monopoly players and their current cash:

```
players = pl.DataFrame(
    {
        "name": ["Alice", "Bob"],
        "cash": [78, 135],
    }
)
print(players)
```
```
let players = df!(
    "name" => ["Alice", "Bob"],
    "cash" => [78, 135],
)?;
println!("{players}");
```
```
shape: (2, 2)
┌───────┬──────┐
│ name  ┆ cash │
│ ---   ┆ ---  │
│ str   ┆ i64  │
╞═══════╪══════╡
│ Alice ┆ 78   │
│ Bob   ┆ 135  │
└───────┴──────┘
```
Using a non-equi join we can easily build a dataframe with all the possible properties that each
player could be interested in buying. We use the function `join_where` to compute a non-equi join:

```
result = players.join_where(props_prices, pl.col("cash") > pl.col("cost"))
print(result)
```
  [`join_where`](https://docs.pola.rs/api/rust/dev/polars/prelude/struct.JoinBuilder.html#method.join_where) ·  [Available on feature iejoin](/user-guide/installation/#feature-flags)

```
let result = players
    .clone()
    .lazy()
    .join_builder()
    .with(props_prices.lazy())
    .join_where(vec![col("cash").cast(DataType::Int64).gt(col("cost"))])
    .collect()?;
println!("{result}");
```
```
shape: (6, 4)
┌───────┬──────┬──────────────────────┬──────┐
│ name  ┆ cash ┆ property_name        ┆ cost │
│ ---   ┆ ---  ┆ ---                  ┆ ---  │
│ str   ┆ i64  ┆ str                  ┆ i64  │
╞═══════╪══════╪══════════════════════╪══════╡
│ Bob   ┆ 135  ┆ Sesame Street        ┆ 100  │
│ Bob   ┆ 135  ┆ The Angel, Islington ┆ 100  │
│ Bob   ┆ 135  ┆ Old Ken Road         ┆ 60   │
│ Bob   ┆ 135  ┆ Whitechapel Road     ┆ 60   │
│ Alice ┆ 78   ┆ Old Ken Road         ┆ 60   │
│ Alice ┆ 78   ┆ Whitechapel Road     ┆ 60   │
└───────┴──────┴──────────────────────┴──────┘
```
You can provide multiple expressions as predicates, in that case they will be AND combined. You can also combine expressions in a single expression if you need other combinations like OR or XOR.

## Asof join

An `asof` join is like a left join except that we match on nearest key rather than equal keys. In
Polars we can do an asof join with the `join_asof` method.

For the asof join we will consider a scenario inspired by the stock market. Suppose a stock market
broker has a dataframe called `df_trades` showing transactions it has made for different stocks.

```
from datetime import datetime
df_trades = pl.DataFrame(
    {
        "time": [
            datetime(2020, 1, 1, 9, 1, 0),
            datetime(2020, 1, 1, 9, 1, 0),
            datetime(2020, 1, 1, 9, 3, 0),
            datetime(2020, 1, 1, 9, 6, 0),
        ],
        "stock": ["A", "B", "B", "C"],
        "trade": [101, 299, 301, 500],
    }
)
print(df_trades)
```
```
use chrono::prelude::*;
let df_trades = df!(
    "time" => [
        NaiveDate::from_ymd_opt(2020, 1, 1).unwrap().and_hms_opt(9, 1, 0).unwrap(),
        NaiveDate::from_ymd_opt(2020, 1, 1).unwrap().and_hms_opt(9, 1, 0).unwrap(),
        NaiveDate::from_ymd_opt(2020, 1, 1).unwrap().and_hms_opt(9, 3, 0).unwrap(),
        NaiveDate::from_ymd_opt(2020, 1, 1).unwrap().and_hms_opt(9, 6, 0).unwrap(),
    ],
    "stock" => ["A", "B", "B", "C"],
    "trade" => [101, 299, 301, 500],
)?;
println!("{df_trades}");
```
```
shape: (4, 3)
┌─────────────────────┬───────┬───────┐
│ time                ┆ stock ┆ trade │
│ ---                 ┆ ---   ┆ ---   │
│ datetime[μs]        ┆ str   ┆ i64   │
╞═════════════════════╪═══════╪═══════╡
│ 2020-01-01 09:01:00 ┆ A     ┆ 101   │
│ 2020-01-01 09:01:00 ┆ B     ┆ 299   │
│ 2020-01-01 09:03:00 ┆ B     ┆ 301   │
│ 2020-01-01 09:06:00 ┆ C     ┆ 500   │
└─────────────────────┴───────┴───────┘
```
The broker has another dataframe called `df_quotes` showing prices it has quoted for these stocks:

```
df_quotes = pl.DataFrame(
    {
        "time": [
            datetime(2020, 1, 1, 9, 0, 0),
            datetime(2020, 1, 1, 9, 2, 0),
            datetime(2020, 1, 1, 9, 4, 0),
            datetime(2020, 1, 1, 9, 6, 0),
        ],
        "stock": ["A", "B", "C", "A"],
        "quote": [100, 300, 501, 102],
    }
)
print(df_quotes)
```
```
let df_quotes = df!(
    "time" => [
        NaiveDate::from_ymd_opt(2020, 1, 1).unwrap().and_hms_opt(9, 1, 0).unwrap(),
        NaiveDate::from_ymd_opt(2020, 1, 1).unwrap().and_hms_opt(9, 2, 0).unwrap(),
        NaiveDate::from_ymd_opt(2020, 1, 1).unwrap().and_hms_opt(9, 4, 0).unwrap(),
        NaiveDate::from_ymd_opt(2020, 1, 1).unwrap().and_hms_opt(9, 6, 0).unwrap(),
    ],
    "stock" => ["A", "B", "C", "A"],
    "quote" => [100, 300, 501, 102],
)?;
println!("{df_quotes}");
```
```
shape: (4, 3)
┌─────────────────────┬───────┬───────┐
│ time                ┆ stock ┆ quote │
│ ---                 ┆ ---   ┆ ---   │
│ datetime[μs]        ┆ str   ┆ i64   │
╞═════════════════════╪═══════╪═══════╡
│ 2020-01-01 09:00:00 ┆ A     ┆ 100   │
│ 2020-01-01 09:02:00 ┆ B     ┆ 300   │
│ 2020-01-01 09:04:00 ┆ C     ┆ 501   │
│ 2020-01-01 09:06:00 ┆ A     ┆ 102   │
└─────────────────────┴───────┴───────┘
```
You want to produce a dataframe showing for each trade the most recent quote provided *on or before*
the time of the trade. You do this with `join_asof` (using the default `strategy = "backward"`). To
avoid joining between trades on one stock with a quote on another you must specify an exact
preliminary join on the stock column with `by="stock"`.

```
df_asof_join = df_trades.join_asof(
    df_quotes, on="time", by="stock", check_sortedness=False
)
print(df_asof_join)
```
  [`join_asof_by`](https://docs.pola.rs/api/rust/dev/polars/prelude/trait.AsofJoinBy.html#method.join_asof_by) ·  [Available on feature asof_join](/user-guide/installation/#feature-flags)

```
let result = df_trades.join_asof_by(
    &df_quotes,
    "time",
    "time",
    ["stock"],
    ["stock"],
    AsofStrategy::Backward,
    None,
    true,
    true,
)?;
println!("{result}");
```
```
shape: (4, 4)
┌─────────────────────┬───────┬───────┬───────┐
│ time                ┆ stock ┆ trade ┆ quote │
│ ---                 ┆ ---   ┆ ---   ┆ ---   │
│ datetime[μs]        ┆ str   ┆ i64   ┆ i64   │
╞═════════════════════╪═══════╪═══════╪═══════╡
│ 2020-01-01 09:01:00 ┆ A     ┆ 101   ┆ 100   │
│ 2020-01-01 09:01:00 ┆ B     ┆ 299   ┆ null  │
│ 2020-01-01 09:03:00 ┆ B     ┆ 301   ┆ 300   │
│ 2020-01-01 09:06:00 ┆ C     ┆ 500   ┆ 501   │
└─────────────────────┴───────┴───────┴───────┘
```
If you want to make sure that only quotes within a certain time range are joined to the trades you
can specify the `tolerance` argument. In this case we want to make sure that the last preceding
quote is within 1 minute of the trade so we set `tolerance = "1m"`.

```
df_asof_tolerance_join = df_trades.join_asof(
    df_quotes, on="time", by="stock", tolerance="1m", check_sortedness=False
)
print(df_asof_tolerance_join)
```
  [`join_asof_by`](https://docs.pola.rs/api/rust/dev/polars/prelude/trait.AsofJoinBy.html#method.join_asof_by) ·  [Available on feature asof_join](/user-guide/installation/#feature-flags)

```
let result = df_trades.join_asof_by(
    &df_quotes,
    "time",
    "time",
    ["stock"],
    ["stock"],
    AsofStrategy::Backward,
    Some(AnyValue::Duration(60000, TimeUnit::Milliseconds)),
    true,
    true,
)?;
println!("{result}");
```
```
shape: (4, 4)
┌─────────────────────┬───────┬───────┬───────┐
│ time                ┆ stock ┆ trade ┆ quote │
│ ---                 ┆ ---   ┆ ---   ┆ ---   │
│ datetime[μs]        ┆ str   ┆ i64   ┆ i64   │
╞═════════════════════╪═══════╪═══════╪═══════╡
│ 2020-01-01 09:01:00 ┆ A     ┆ 101   ┆ 100   │
│ 2020-01-01 09:01:00 ┆ B     ┆ 299   ┆ null  │
│ 2020-01-01 09:03:00 ┆ B     ┆ 301   ┆ 300   │
│ 2020-01-01 09:06:00 ┆ C     ┆ 500   ┆ null  │
└─────────────────────┴───────┴───────┴───────┘
```
## Cartesian product

Polars allows you to compute the
[Cartesian product](https://en.wikipedia.org/wiki/Cartesian_product) of two dataframes, producing a
dataframe where all rows of the left dataframe are paired up with all the rows of the right
dataframe. To compute the Cartesian product of two dataframes, you can pass the strategy
`how="cross"` to the function `join` without specifying any of `on`, `left_on`, and `right_on`:

```
tokens = pl.DataFrame({"monopoly_token": ["hat", "shoe", "boat"]})
result = players.select(pl.col("name")).join(tokens, how="cross")
print(result)
```
  [`cross_join`](https://docs.pola.rs/api/rust/dev/polars/prelude/struct.LazyFrame.html#method.cross_join) ·  [Available on feature cross_join](/user-guide/installation/#feature-flags)

```
let tokens = df!(
    "monopoly_token" => ["hat", "shoe", "boat"],
)?;
let result = players
    .lazy()
    .select([col("name")])
    .cross_join(tokens.lazy(), None)
    .collect()?;
println!("{result}");
```
```
shape: (6, 2)
┌───────┬────────────────┐
│ name  ┆ monopoly_token │
│ ---   ┆ ---            │
│ str   ┆ str            │
╞═══════╪════════════════╡
│ Alice ┆ hat            │
│ Alice ┆ shoe           │
│ Alice ┆ boat           │
│ Bob   ┆ hat            │
│ Bob   ┆ shoe           │
│ Bob   ┆ boat           │
└───────┴────────────────┘
```

# Citations

1. Source page: https://docs.pola.rs/user-guide/transformations/joins
