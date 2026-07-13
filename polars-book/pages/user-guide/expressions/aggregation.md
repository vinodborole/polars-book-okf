---
type: Web Page
title: Aggregation - Polars user guide
resource: https://docs.pola.rs/user-guide/expressions/aggregation
timestamp: '2026-07-13T09:31:11.337600+00:00'
---

# Aggregation

The Polars [context](../../concepts/expressions-and-contexts/#contexts) `group_by` lets you apply
expressions on subsets of columns, as defined by the unique values of the column over which the data
is grouped. This is a very powerful capability that we explore in this section of the user guide.

We start by reading in a
[US congress  dataset](https://github.com/unitedstates/congress-legislators):

```
import polars as pl
url = "hf://datasets/nameexhaustion/polars-docs/legislators-historical.csv"
schema_overrides = {
    "first_name": pl.Categorical,
    "gender": pl.Categorical,
    "type": pl.Categorical,
    "state": pl.Categorical,
    "party": pl.Categorical,
}
dataset = (
    pl.read_csv(url, schema_overrides=schema_overrides)
    .with_columns(pl.col("first", "middle", "last").name.suffix("_name"))
    .with_columns(pl.col("birthday").str.to_date(strict=False))
)
```
[   DataFrame](https://docs.pola.rs/api/rust/dev/polars/frame/struct.DataFrame.html) ·

[·](https://docs.pola.rs/api/rust/dev/polars/prelude/enum.DataType.html#variant.Categorical)

`Categorical`[Available on feature dtype-categorical](/user-guide/installation/#feature-flags)

```
use std::io::Cursor;
use polars::prelude::*;
use reqwest::blocking::Client;
let url = "https://huggingface.co/datasets/nameexhaustion/polars-docs/resolve/main/legislators-historical.csv";
let mut schema = Schema::default();
schema.with_column(
    "first_name".into(),
    DataType::from_categories(Categories::global()),
);
schema.with_column(
    "gender".into(),
    DataType::from_categories(Categories::global()),
);
schema.with_column(
    "type".into(),
    DataType::from_categories(Categories::global()),
);
schema.with_column(
    "state".into(),
    DataType::from_categories(Categories::global()),
);
schema.with_column(
    "party".into(),
    DataType::from_categories(Categories::global()),
);
schema.with_column("birthday".into(), DataType::Date);
let data = Client::new().get(url).send()?.bytes()?;
let dataset = CsvReadOptions::default()
    .with_has_header(true)
    .with_schema_overwrite(Some(Arc::new(schema)))
    .map_parse_options(|parse_options| parse_options.with_try_parse_dates(true))
    .into_reader_with_file_handle(Cursor::new(data))
    .finish()?
    .lazy()
    .with_columns([
        col("first").name().suffix("_name"),
        col("middle").name().suffix("_name"),
        col("last").name().suffix("_name"),
    ])
    .collect()?;
println!("{}", &dataset);
```
## Basic aggregations

You can easily apply multiple expressions to your aggregated values. Simply list all of the
expressions you want inside the function `agg`. There is no upper bound on the number of
aggregations you can do and you can make any combination you want. In the snippet below we will
group the data based on the column “first_name” and then we will apply the following aggregations:

- count the number of rows in the group (which means we count how many people in the data set have each unique first name);
- combine the values of the column “gender” into a list by referring the column but omitting an aggregate function; and
- get the first value of the column “last_name” within the group.

After computing the aggregations, we immediately sort the result and limit it to the top five rows so that we have a nice summary overview:

```
q = (
    dataset.lazy()
    .group_by("first_name")
    .agg(
        pl.len(),
        pl.col("gender"),
        pl.first("last_name"),  # Short for `pl.col("last_name").first()`
    )
    .sort("len", descending=True)
    .limit(5)
)
df = q.collect()
print(df)
```
```
let df = dataset
    .clone()
    .lazy()
    .group_by(["first_name"])
    .agg([len(), col("gender"), col("last_name").first()])
    .sort(
        ["len"],
        SortMultipleOptions::default()
            .with_order_descending(true)
            .with_nulls_last(true),
    )
    .limit(5)
    .collect()?;
println!("{df}");
```
```
shape: (5, 4)
┌────────────┬──────┬───────────────────┬───────────┐
│ first_name ┆ len  ┆ gender            ┆ last_name │
│ ---        ┆ ---  ┆ ---               ┆ ---       │
│ str        ┆ u32  ┆ list[cat]         ┆ str       │
╞════════════╪══════╪═══════════════════╪═══════════╡
│ John       ┆ 4227 ┆ ["M", "M", … "M"] ┆ Walker    │
│ William    ┆ 3309 ┆ ["M", "M", … "M"] ┆ Few       │
│ James      ┆ 2414 ┆ ["M", "M", … "M"] ┆ Armstrong │
│ Charles    ┆ 1514 ┆ ["M", "M", … "M"] ┆ Carroll   │
│ Thomas     ┆ 1502 ┆ ["M", "M", … "M"] ┆ Tucker    │
└────────────┴──────┴───────────────────┴───────────┘
```
It's that easy! Let's turn it up a notch.

## Conditionals

Let's say we want to know how many delegates of a state are “Pro” or “Anti” administration. We can
query that directly in the aggregation without the need for a `lambda` or grooming the dataframe:

```
q = (
    dataset.lazy()
    .group_by("state")
    .agg(
        (pl.col("party") == "Anti-Administration").sum().alias("anti"),
        (pl.col("party") == "Pro-Administration").sum().alias("pro"),
    )
    .sort("pro", descending=True)
    .limit(5)
)
df = q.collect()
print(df)
```
```
let df = dataset
    .clone()
    .lazy()
    .group_by(["state"])
    .agg([
        (col("party").eq(lit("Anti-Administration")))
            .sum()
            .alias("anti"),
        (col("party").eq(lit("Pro-Administration")))
            .sum()
            .alias("pro"),
    ])
    .sort(
        ["pro"],
        SortMultipleOptions::default().with_order_descending(true),
    )
    .limit(5)
    .collect()?;
println!("{df}");
```
```
shape: (5, 3)
┌───────┬──────┬─────┐
│ state ┆ anti ┆ pro │
│ ---   ┆ ---  ┆ --- │
│ cat   ┆ u32  ┆ u32 │
╞═══════╪══════╪═════╡
│ CT    ┆ 0    ┆ 5   │
│ NJ    ┆ 0    ┆ 3   │
│ DE    ┆ 1    ┆ 3   │
│ MA    ┆ 0    ┆ 2   │
│ MD    ┆ 0    ┆ 2   │
└───────┴──────┴─────┘
```
## Filtering

We can also filter the groups. Let's say we want to compute a mean per group, but we don't want to include all values from that group, and we also don't want to actually filter the rows from the dataframe because we need those rows for another aggregation.

In the example below we show how this can be done.

Note

Note that we can define Python functions for clarity. These functions don't cost us anything because they return Polars expressions, we don't apply a custom function over a series during runtime of the query. Of course, you can write functions that return expressions in Rust, too.

```
from datetime import date
def compute_age():
    return date.today().year - pl.col("birthday").dt.year()
def avg_age(gender: str) -> pl.Expr:
    return (
        compute_age()
        .filter(pl.col("gender") == gender)
        .mean()
        .alias(f"avg {gender} age")
    )
q = (
    dataset.lazy()
    .group_by("state")
    .agg(
        avg_age("M"),
        avg_age("F"),
        (pl.col("gender") == "M").sum().alias("# male"),
        (pl.col("gender") == "F").sum().alias("# female"),
    )
    .limit(5)
)
df = q.collect()
print(df)
```
```
fn compute_age() -> Expr {
    lit(2024) - col("birthday").dt().year()
}
fn avg_birthday(gender: &str) -> Expr {
    compute_age()
        .filter(col("gender").eq(lit(gender)))
        .mean()
        .alias(format!("avg {gender} birthday"))
}
let df = dataset
    .clone()
    .lazy()
    .group_by(["state"])
    .agg([
        avg_birthday("M"),
        avg_birthday("F"),
        (col("gender").eq(lit("M"))).sum().alias("# male"),
        (col("gender").eq(lit("F"))).sum().alias("# female"),
    ])
    .limit(5)
    .collect()?;
println!("{df}");
```
```
shape: (5, 5)
┌───────┬────────────┬───────────┬────────┬──────────┐
│ state ┆ avg M age  ┆ avg F age ┆ # male ┆ # female │
│ ---   ┆ ---        ┆ ---       ┆ ---    ┆ ---      │
│ cat   ┆ f64        ┆ f64       ┆ u32    ┆ u32      │
╞═══════╪════════════╪═══════════╪════════╪══════════╡
│ WY    ┆ 139.552632 ┆ 74.0      ┆ 114    ┆ 10       │
│ HI    ┆ 116.321839 ┆ 88.6      ┆ 87     ┆ 25       │
│ PA    ┆ 167.008592 ┆ 92.636364 ┆ 3205   ┆ 22       │
│ PI    ┆ 149.219512 ┆ null      ┆ 41     ┆ 0        │
│ IL    ┆ 146.069482 ┆ 99.203704 ┆ 1895   ┆ 54       │
└───────┴────────────┴───────────┴────────┴──────────┘
```
Do the average age values look nonsensical? That's because we are working with historical data that dates back to the 1800s and we are doing our computations assuming everyone represented in the dataset is still alive and kicking.

## Nested grouping

The two previous queries could have been done with a nested `group_by`, but that wouldn't have let
us show off some of these features. 😉 To do a nested `group_by`, simply list the columns that will
be used for grouping.

First, we use a nested `group_by` to figure out how many delegates of a state are “Pro” or “Anti”
administration:

```
q = (
    dataset.lazy()
    .group_by("state", "party")
    .agg(pl.len().alias("count"))
    .filter(
        (pl.col("party") == "Anti-Administration")
        | (pl.col("party") == "Pro-Administration")
    )
    .sort("count", descending=True)
    .limit(5)
)
df = q.collect()
print(df)
```
```
let df = dataset
    .clone()
    .lazy()
    .group_by(["state", "party"])
    .agg([len().alias("count")])
    .filter(
        col("party")
            .eq(lit("Anti-Administration"))
            .or(col("party").eq(lit("Pro-Administration"))),
    )
    .sort(
        ["count"],
        SortMultipleOptions::default()
            .with_order_descending(true)
            .with_nulls_last(true),
    )
    .limit(5)
    .collect()?;
println!("{df}");
```
```
shape: (5, 3)
┌───────┬─────────────────────┬───────┐
│ state ┆ party               ┆ count │
│ ---   ┆ ---                 ┆ ---   │
│ cat   ┆ cat                 ┆ u32   │
╞═══════╪═════════════════════╪═══════╡
│ VA    ┆ Anti-Administration ┆ 5     │
│ CT    ┆ Pro-Administration  ┆ 5     │
│ PA    ┆ Anti-Administration ┆ 3     │
│ DE    ┆ Pro-Administration  ┆ 3     │
│ NJ    ┆ Pro-Administration  ┆ 3     │
└───────┴─────────────────────┴───────┘
```
Next, we use a nested `group_by` to compute the average age of delegates per state and per gender:

```
q = (
    dataset.lazy()
    .group_by("state", "gender")
    .agg(
        # The function `avg_age` is not needed:
        compute_age().mean().alias("avg age"),
        pl.len().alias("#"),
    )
    .sort("#", descending=True)
    .limit(5)
)
df = q.collect()
print(df)
```
```
let df = dataset
    .clone()
    .lazy()
    .group_by(["state", "gender"])
    .agg([compute_age().mean().alias("avg birthday"), len().alias("#")])
    .sort(
        ["#"],
        SortMultipleOptions::default()
            .with_order_descending(true)
            .with_nulls_last(true),
    )
    .limit(5)
    .collect()?;
println!("{df}");
```
```
shape: (5, 4)
┌───────┬────────┬────────────┬──────┐
│ state ┆ gender ┆ avg age    ┆ #    │
│ ---   ┆ ---    ┆ ---        ┆ ---  │
│ cat   ┆ cat    ┆ f64        ┆ u32  │
╞═══════╪════════╪════════════╪══════╡
│ NY    ┆ M      ┆ 165.204634 ┆ 3965 │
│ PA    ┆ M      ┆ 167.008592 ┆ 3205 │
│ OH    ┆ M      ┆ 157.579961 ┆ 2142 │
│ IL    ┆ M      ┆ 146.069482 ┆ 1895 │
│ CA    ┆ M      ┆ 115.400464 ┆ 1725 │
└───────┴────────┴────────────┴──────┘
```
Note that we get the same results but the format of the data is different. Depending on the situation, one format may be more suitable than the other.

## Sorting

It is common to see a dataframe being sorted for the sole purpose of managing the ordering during a grouping operation. Let's say that we want to get the names of the oldest and youngest politicians per state. We could start by sorting and then grouping:

```
def get_name() -> pl.Expr:
    return pl.col("first_name") + pl.lit(" ") + pl.col("last_name")
q = (
    dataset.lazy()
    .sort("birthday", descending=True)
    .group_by("state")
    .agg(
        get_name().first().alias("youngest"),
        get_name().last().alias("oldest"),
    )
    .limit(5)
)
df = q.collect()
print(df)
```
```
fn get_name() -> Expr {
    col("first_name") + lit(" ") + col("last_name")
}
let df = dataset
    .clone()
    .lazy()
    .sort(
        ["birthday"],
        SortMultipleOptions::default()
            .with_order_descending(true)
            .with_nulls_last(true),
    )
    .group_by(["state"])
    .agg([
        get_name().first().alias("youngest"),
        get_name().last().alias("oldest"),
    ])
    .limit(5)
    .collect()?;
println!("{df}");
```
```
shape: (5, 3)
┌───────┬───────────────────┬──────────────────┐
│ state ┆ youngest          ┆ oldest           │
│ ---   ┆ ---               ┆ ---              │
│ cat   ┆ str               ┆ str              │
╞═══════╪═══════════════════╪══════════════════╡
│ RI    ┆ James Mason       ┆ William Bradford │
│ VI    ┆ Donna Christensen ┆ Melvin Evans     │
│ HI    ┆ Tulsi Gabbard     ┆ Robert Wilcox    │
│ TN    ┆ William Cocke     ┆ John Sevier      │
│ NH    ┆ John Sherburne    ┆ Samuel Livermore │
└───────┴───────────────────┴──────────────────┘
```
However, if we also want to sort the names alphabetically, we need to perform an extra sort
operation. Luckily, we can sort in a `group_by` context without changing the sorting of the
underlying dataframe:

```
q = (
    dataset.lazy()
    .sort("birthday", descending=True)
    .group_by("state")
    .agg(
        get_name().first().alias("youngest"),
        get_name().last().alias("oldest"),
        get_name().sort().first().alias("alphabetical_first"),
    )
    .limit(5)
)
df = q.collect()
print(df)
```
```
let df = dataset
    .clone()
    .lazy()
    .sort(
        ["birthday"],
        SortMultipleOptions::default()
            .with_order_descending(true)
            .with_nulls_last(true),
    )
    .group_by(["state"])
    .agg([
        get_name().first().alias("youngest"),
        get_name().last().alias("oldest"),
        get_name()
            .sort(Default::default())
            .first()
            .alias("alphabetical_first"),
    ])
    .limit(5)
    .collect()?;
println!("{df}");
```
```
shape: (5, 4)
┌───────┬────────────────┬──────────────────┬────────────────────┐
│ state ┆ youngest       ┆ oldest           ┆ alphabetical_first │
│ ---   ┆ ---            ┆ ---              ┆ ---                │
│ cat   ┆ str            ┆ str              ┆ str                │
╞═══════╪════════════════╪══════════════════╪════════════════════╡
│ ME    ┆ Samuel Butman  ┆ Joshua Cushman   ┆ Albert Smith       │
│ NH    ┆ John Sherburne ┆ Samuel Livermore ┆ Aaron Cragin       │
│ DE    ┆ Samuel White   ┆ George Read      ┆ Albert Polk        │
│ DK    ┆ George Mathews ┆ John Todd        ┆ George Mathews     │
│ CA    ┆ Edward Gilbert ┆ William Gwin     ┆ Aaron Sargent      │
└───────┴────────────────┴──────────────────┴────────────────────┘
```
We can even sort a column with the order induced by another column, and this also works inside the
context `group_by`. This modification to the previous query lets us check if the delegate with the
first name is male or female:

```
q = (
    dataset.lazy()
    .sort("birthday", descending=True)
    .group_by("state")
    .agg(
        get_name().first().alias("youngest"),
        get_name().last().alias("oldest"),
        get_name().sort().first().alias("alphabetical_first"),
        pl.col("gender").sort_by(get_name()).first(),
    )
    .sort("state")
    .limit(5)
)
df = q.collect()
print(df)
```
```
let df = dataset
    .lazy()
    .sort(
        ["birthday"],
        SortMultipleOptions::default()
            .with_order_descending(true)
            .with_nulls_last(true),
    )
    .group_by(["state"])
    .agg([
        get_name().first().alias("youngest"),
        get_name().last().alias("oldest"),
        get_name()
            .sort(Default::default())
            .first()
            .alias("alphabetical_first"),
        col("gender")
            .sort_by(["first_name"], SortMultipleOptions::default())
            .first(),
    ])
    .sort(["state"], SortMultipleOptions::default())
    .limit(5)
    .collect()?;
println!("{df}");
```
```
shape: (5, 5)
┌───────┬──────────────────┬────────────────┬────────────────────┬────────┐
│ state ┆ youngest         ┆ oldest         ┆ alphabetical_first ┆ gender │
│ ---   ┆ ---              ┆ ---            ┆ ---                ┆ ---    │
│ cat   ┆ str              ┆ str            ┆ str                ┆ cat    │
╞═══════╪══════════════════╪════════════════╪════════════════════╪════════╡
│ AK    ┆ Mary Peltola     ┆ Thomas Cale    ┆ Anthony Dimond     ┆ M      │
│ AL    ┆ John McKee       ┆ Israel Pickens ┆ Albert Goodwyn     ┆ M      │
│ AR    ┆ Archibald Yell   ┆ James Bates    ┆ Albert Rust        ┆ M      │
│ AS    ┆ Eni Faleomavaega ┆ Fofó Sunia     ┆ Eni Faleomavaega   ┆ M      │
│ AZ    ┆ Ben Quayle       ┆ Coles Bashford ┆ Ann Kirkpatrick    ┆ F      │
└───────┴──────────────────┴────────────────┴────────────────────┴────────┘
```
## Do not kill parallelization

Python users only

The following section is specific to Python, and doesn't apply to Rust. Within Rust, blocks and closures (lambdas) can, and will, be executed concurrently.

Python is generally slower than Rust. Besides the overhead of running “slow” bytecode, Python has to
remain within the constraints of the Global Interpreter Lock (GIL). This means that if you were to
use a `lambda` or a custom Python function to apply during a parallelized phase, Polars' speed is
capped running Python code, preventing any multiple threads from executing the function.

Polars will try to parallelize the computation of the aggregating functions over the groups, so it
is recommended that you avoid using `lambda`s and custom Python functions as much as possible.
Instead, try to stay within the realm of the Polars expression API. This is not always possible,
though, so if you want to learn more about using `lambda`s you can go
[the user guide section on using user-defined functions](../user-defined-python-functions/).

# Citations

1. Source page: https://docs.pola.rs/user-guide/expressions/aggregation
