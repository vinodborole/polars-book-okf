---
type: Web Page
title: Introduction - Polars user guide
resource: https://docs.pola.rs/user-guide/sql/intro
timestamp: '2026-07-07T12:26:19.464100+00:00'
---

# Introduction

While Polars supports interaction with SQL, it's recommended that users familiarize themselves with the expression syntax to produce more readable and expressive code. As the DataFrame interface is primary, new features are typically added to the expression API first. However, if you already have an existing SQL codebase or prefer the use of SQL, Polars does offers support for this.

Note

There is no separate SQL engine because Polars translates SQL queries into expressions, which are then executed using its own engine. This approach ensures that Polars maintains its performance and scalability advantages as a native DataFrame library, while still providing users with the ability to work with SQL.

## Context

Polars uses the `SQLContext` object to manage SQL queries. The context contains a mapping of
`DataFrame` and `LazyFrame` identifier names to their corresponding datasets1. The example below
starts a `SQLContext`:

```
ctx = pl.SQLContext()
```
## Register Dataframes

There are several ways to register DataFrames during `SQLContext` initialization.

- register all `LazyFrame`and`DataFrame`objects in the global namespace.
- register explicitly via a dictionary mapping, or kwargs.

```
df = pl.DataFrame({"a": [1, 2, 3]})
lf = pl.LazyFrame({"b": [4, 5, 6]})
# Register all dataframes in the global namespace: registers both "df" and "lf"
ctx = pl.SQLContext(register_globals=True)
# Register an explicit mapping of identifier name to frame
ctx = pl.SQLContext(frames={"table_one": df, "table_two": lf})
# Register frames using kwargs; dataframe df as "df" and lazyframe lf as "lf"
ctx = pl.SQLContext(df=df, lf=lf)
```
We can also register Pandas DataFrames by converting them to Polars first.

```
import pandas as pd
df_pandas = pd.DataFrame({"c": [7, 8, 9]})
ctx = pl.SQLContext(df_pandas=pl.from_pandas(df_pandas))
```
Note

Converting a Pandas DataFrame backed by Numpy will trigger a potentially expensive conversion; however, if the Pandas DataFrame is already backed by Arrow then the conversion will be significantly cheaper (and in some cases close to free).

Once the `SQLContext` is initialized, we can register additional Dataframes or unregister existing
Dataframes with:

- `register`
- `register_globals`
- `register_many`
- `unregister`

## Execute queries and collect results

SQL queries are always executed in lazy mode to take advantage of the full set of query planning optimizations, so we have two options to collect the result:

- Set the parameter `eager_execution`to True in`SQLContext`; this ensures that Polars automatically collects the LazyFrame results from`execute`calls.
- Set the parameter `eager`to True when executing a query with`execute`, or explicitly collect the result using`collect`.

We execute SQL queries by calling `execute` on a `SQLContext`.

```
shape: (5, 13)
┌─────┬───────────────────────┬────────┬────────┬───┬─────────┬───────┬────────────┬───────────┐
│ #   ┆ Name                  ┆ Type 1 ┆ Type 2 ┆ … ┆ Sp. Def ┆ Speed ┆ Generation ┆ Legendary │
│ --- ┆ ---                   ┆ ---    ┆ ---    ┆   ┆ ---     ┆ ---   ┆ ---        ┆ ---       │
│ i64 ┆ str                   ┆ str    ┆ str    ┆   ┆ i64     ┆ i64   ┆ i64        ┆ bool      │
╞═════╪═══════════════════════╪════════╪════════╪═══╪═════════╪═══════╪════════════╪═══════════╡
│ 1   ┆ Bulbasaur             ┆ Grass  ┆ Poison ┆ … ┆ 65      ┆ 45    ┆ 1          ┆ false     │
│ 2   ┆ Ivysaur               ┆ Grass  ┆ Poison ┆ … ┆ 80      ┆ 60    ┆ 1          ┆ false     │
│ 3   ┆ Venusaur              ┆ Grass  ┆ Poison ┆ … ┆ 100     ┆ 80    ┆ 1          ┆ false     │
│ 3   ┆ VenusaurMega Venusaur ┆ Grass  ┆ Poison ┆ … ┆ 120     ┆ 80    ┆ 1          ┆ false     │
│ 4   ┆ Charmander            ┆ Fire   ┆ null   ┆ … ┆ 50      ┆ 65    ┆ 1          ┆ false     │
└─────┴───────────────────────┴────────┴────────┴───┴─────────┴───────┴────────────┴───────────┘
```
## Execute queries from multiple sources

SQL queries can be executed just as easily from multiple sources. In the example below, we register:

- a CSV file (loaded lazily)
- a NDJSON file (loaded lazily)
- a Pandas DataFrame

And join them together using SQL. Lazy reading lets you only load the necessary rows and columns from the files.

In the same way, it's possible to register cloud datalakes (S3, Azure Data Lake). A PyArrow dataset
can point to the datalake, then Polars can read it with `scan_pyarrow_dataset`.

```
# Input data:
# products_masterdata.csv with schema {'product_id': Int64, 'product_name': String}
# products_categories.json with schema {'product_id': Int64, 'category': String}
# sales_data is a Pandas DataFrame with schema {'product_id': Int64, 'sales': Int64}
with pl.SQLContext(
    products_masterdata=pl.scan_csv("docs/assets/data/products_masterdata.csv"),
    products_categories=pl.scan_ndjson("docs/assets/data/products_categories.json"),
    sales_data=pl.from_pandas(sales_data),
    eager=True,
) as ctx:
    query = """
    SELECT
        product_id,
        product_name,
        category,
        sales
    FROM
        products_masterdata
    LEFT JOIN products_categories USING (product_id)
    LEFT JOIN sales_data USING (product_id)
    """
    print(ctx.execute(query))
```
```
shape: (5, 4)
┌────────────┬──────────────┬────────────┬───────┐
│ product_id ┆ product_name ┆ category   ┆ sales │
│ ---        ┆ ---          ┆ ---        ┆ ---   │
│ i64        ┆ str          ┆ str        ┆ i64   │
╞════════════╪══════════════╪════════════╪═══════╡
│ 1          ┆ Product A    ┆ Category 1 ┆ 100   │
│ 2          ┆ Product B    ┆ Category 1 ┆ 200   │
│ 3          ┆ Product C    ┆ Category 2 ┆ 150   │
│ 4          ┆ Product D    ┆ Category 2 ┆ 250   │
│ 5          ┆ Product E    ┆ Category 3 ┆ 300   │
└────────────┴──────────────┴────────────┴───────┘
```
## Compatibility

Polars does not support the complete SQL specification, but it does support a subset of the most common statement types.

Note

Where possible, Polars aims to follow PostgreSQL syntax definitions and function behaviour.

For example, here is a non-exhaustive list of some of the supported functionality:

- Write a `CREATE`statements:`CREATE TABLE xxx AS ...`
- Write a `SELECT`statements containing:`WHERE`,`ORDER`,`LIMIT`,`GROUP BY`,`UNION`and`JOIN`clauses ...
- Write Common Table Expressions (CTE's) such as: `WITH tablename AS`
- Explain a query: `EXPLAIN SELECT ...`
- List registered tables: `SHOW TABLES`
- Drop a table: `DROP TABLE [IF EXISTS] tablename`
- Truncate a table: `TRUNCATE TABLE [IF EXISTS] tablename`

The following are some features that are not yet supported:

- `INSERT`,- `UPDATE`or- `DELETE`statements
- Meta queries such as `ANALYZE`

In the upcoming sections we will cover each of the statements in more detail.

- 
Additionally it also tracks the common table expressions as well. ↩

# Citations

1. Source page: https://docs.pola.rs/user-guide/sql/intro
