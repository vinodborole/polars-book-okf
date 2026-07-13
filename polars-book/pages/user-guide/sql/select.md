---
type: Web Page
title: SELECT - Polars user guide
resource: https://docs.pola.rs/user-guide/sql/select
timestamp: '2026-07-13T09:31:11.337600+00:00'
---

# SELECT

In Polars SQL, the `SELECT` statement is used to retrieve data from a table into a `DataFrame`. The
basic syntax of a `SELECT` statement in Polars SQL is as follows:

```
SELECT column1, column2, ...
FROM table_name;
```
Here, `column1`, `column2`, etc. are the columns that you want to select from the table. You can
also use the wildcard `*` to select all columns. `table_name` is the name of the table or that you
want to retrieve data from. In the sections below we will cover some of the more common SELECT
variants

```
df = pl.DataFrame(
    {
        "country": ["USA", "USA", "USA", "USA", "USA", "Netherlands"],
        "city": [
            "New York",
            "Los Angeles",
            "Chicago",
            "Houston",
            "Phoenix",
            "Amsterdam",
        ],
        "population": [8399000, 3997000, 2705000, 2320000, 1680000, 900000],
    }
)
ctx = pl.SQLContext(population=df, eager=True)
print(ctx.execute("SELECT * FROM population"))
```
```
shape: (6, 3)
┌─────────────┬─────────────┬────────────┐
│ country     ┆ city        ┆ population │
│ ---         ┆ ---         ┆ ---        │
│ str         ┆ str         ┆ i64        │
╞═════════════╪═════════════╪════════════╡
│ USA         ┆ New York    ┆ 8399000    │
│ USA         ┆ Los Angeles ┆ 3997000    │
│ USA         ┆ Chicago     ┆ 2705000    │
│ USA         ┆ Houston     ┆ 2320000    │
│ USA         ┆ Phoenix     ┆ 1680000    │
│ Netherlands ┆ Amsterdam   ┆ 900000     │
└─────────────┴─────────────┴────────────┘
```
### GROUP BY

The `GROUP BY` statement is used to group rows in a table by one or more columns and compute
aggregate functions on each group.

```
result = ctx.execute(
    """
        SELECT country, AVG(population) as avg_population
        FROM population
        GROUP BY country
    """
)
print(result)
```
```
shape: (2, 2)
┌─────────────┬────────────────┐
│ country     ┆ avg_population │
│ ---         ┆ ---            │
│ str         ┆ f64            │
╞═════════════╪════════════════╡
│ Netherlands ┆ 900000.0       │
│ USA         ┆ 3.8202e6       │
└─────────────┴────────────────┘
```
### ORDER BY

The `ORDER BY` statement is used to sort the result set of a query by one or more columns in
ascending or descending order.

```
result = ctx.execute(
    """
        SELECT city, population
        FROM population
        ORDER BY population
    """
)
print(result)
```
```
shape: (6, 2)
┌─────────────┬────────────┐
│ city        ┆ population │
│ ---         ┆ ---        │
│ str         ┆ i64        │
╞═════════════╪════════════╡
│ Amsterdam   ┆ 900000     │
│ Phoenix     ┆ 1680000    │
│ Houston     ┆ 2320000    │
│ Chicago     ┆ 2705000    │
│ Los Angeles ┆ 3997000    │
│ New York    ┆ 8399000    │
└─────────────┴────────────┘
```
### JOIN

```
income = pl.DataFrame(
    {
        "country": [
            "USA",
            "USA",
            "USA",
            "USA",
            "Netherlands",
            "Netherlands",
            "Netherlands",
        ],
        "city": [
            "New York",
            "Los Angeles",
            "Chicago",
            "Houston",
            "Amsterdam",
            "Rotterdam",
            "Utrecht",
        ],
        "income": [55000, 62000, 48000, 52000, 42000, 38000, 41000],
    }
)
ctx.register_many(income=income)
result = ctx.execute(
    """
        SELECT income.*, population.population
        FROM population
        LEFT JOIN income ON population.city = income.city
    """
)
print(result)
```
```
shape: (6, 4)
┌─────────────┬─────────────┬────────┬────────────┐
│ country     ┆ city        ┆ income ┆ population │
│ ---         ┆ ---         ┆ ---    ┆ ---        │
│ str         ┆ str         ┆ i64    ┆ i64        │
╞═════════════╪═════════════╪════════╪════════════╡
│ USA         ┆ New York    ┆ 55000  ┆ 8399000    │
│ USA         ┆ Los Angeles ┆ 62000  ┆ 3997000    │
│ USA         ┆ Chicago     ┆ 48000  ┆ 2705000    │
│ USA         ┆ Houston     ┆ 52000  ┆ 2320000    │
│ null        ┆ null        ┆ null   ┆ 1680000    │
│ Netherlands ┆ Amsterdam   ┆ 42000  ┆ 900000     │
└─────────────┴─────────────┴────────┴────────────┘
```
### Functions

Polars provides a wide range of SQL functions, including:

- Mathematical functions: `ABS`,`EXP`,`LOG`,`ASIN`,`ACOS`,`ATAN`, etc.
- String functions: `LOWER`,`UPPER`,`LTRIM`,`RTRIM`,`STARTS_WITH`,`ENDS_WITH`.
- Aggregation functions: `SUM`,`AVG`,`MIN`,`MAX`,`COUNT`,`STDDEV`,`FIRST`etc.
- Array functions: `EXPLODE`,`UNNEST`,`ARRAY_SUM`,`ARRAY_REVERSE`, etc.

For a full list of supported functions go the
[API documentation](https://docs.rs/polars-sql/latest/src/polars_sql/keywords.rs.html). The example
below demonstrates how to use a function in a query

```
result = ctx.execute(
    """
        SELECT city, population
        FROM population
        WHERE STARTS_WITH(country,'U')
    """
)
print(result)
```
```
shape: (5, 2)
┌─────────────┬────────────┐
│ city        ┆ population │
│ ---         ┆ ---        │
│ str         ┆ i64        │
╞═════════════╪════════════╡
│ New York    ┆ 8399000    │
│ Los Angeles ┆ 3997000    │
│ Chicago     ┆ 2705000    │
│ Houston     ┆ 2320000    │
│ Phoenix     ┆ 1680000    │
└─────────────┴────────────┘
```
### Table Functions

In the examples earlier we first generated a DataFrame which we registered in the `SQLContext`.
Polars also support directly reading from CSV, Parquet, JSON and IPC in your SQL query using table
functions `read_xxx`.

```
result = ctx.execute(
    """
        SELECT *
        FROM read_csv('docs/assets/data/iris.csv')
    """
)
print(result)
```
```
shape: (150, 5)
┌──────────────┬─────────────┬──────────────┬─────────────┬───────────┐
│ sepal_length ┆ sepal_width ┆ petal_length ┆ petal_width ┆ species   │
│ ---          ┆ ---         ┆ ---          ┆ ---         ┆ ---       │
│ f64          ┆ f64         ┆ f64          ┆ f64         ┆ str       │
╞══════════════╪═════════════╪══════════════╪═════════════╪═══════════╡
│ 5.1          ┆ 3.5         ┆ 1.4          ┆ 0.2         ┆ Setosa    │
│ 4.9          ┆ 3.0         ┆ 1.4          ┆ 0.2         ┆ Setosa    │
│ 4.7          ┆ 3.2         ┆ 1.3          ┆ 0.2         ┆ Setosa    │
│ 4.6          ┆ 3.1         ┆ 1.5          ┆ 0.2         ┆ Setosa    │
│ 5.0          ┆ 3.6         ┆ 1.4          ┆ 0.2         ┆ Setosa    │
│ …            ┆ …           ┆ …            ┆ …           ┆ …         │
│ 6.7          ┆ 3.0         ┆ 5.2          ┆ 2.3         ┆ Virginica │
│ 6.3          ┆ 2.5         ┆ 5.0          ┆ 1.9         ┆ Virginica │
│ 6.5          ┆ 3.0         ┆ 5.2          ┆ 2.0         ┆ Virginica │
│ 6.2          ┆ 3.4         ┆ 5.4          ┆ 2.3         ┆ Virginica │
│ 5.9          ┆ 3.0         ┆ 5.1          ┆ 1.8         ┆ Virginica │
└──────────────┴─────────────┴──────────────┴─────────────┴───────────┘
```

# Citations

1. Source page: https://docs.pola.rs/user-guide/sql/select
