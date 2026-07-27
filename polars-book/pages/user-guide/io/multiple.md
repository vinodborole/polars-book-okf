---
type: Web Page
title: Multiple - Polars user guide
resource: https://docs.pola.rs/user-guide/io/multiple
timestamp: '2026-07-27T09:55:25.099201+00:00'
---

# Multiple

## Dealing with multiple files.

Polars can deal with multiple files differently depending on your needs and memory strain.

Let's create some files to give us some context:

```
import polars as pl
df = pl.DataFrame({"foo": [1, 2, 3], "bar": [None, "ham", "spam"]})
for i in range(5):
    df.write_csv(f"docs/assets/data/my_many_files_{i}.csv")
```
## Reading into a single `DataFrame`

To read multiple files into a single `DataFrame`, we can use globbing patterns:

```
df = pl.read_csv("docs/assets/data/my_many_files_*.csv")
print(df)
```
```
shape: (15, 2)
┌─────┬──────┐
│ foo ┆ bar  │
│ --- ┆ ---  │
│ i64 ┆ str  │
╞═════╪══════╡
│ 1   ┆ null │
│ 2   ┆ ham  │
│ 3   ┆ spam │
│ 1   ┆ null │
│ 2   ┆ ham  │
│ …   ┆ …    │
│ 2   ┆ ham  │
│ 3   ┆ spam │
│ 1   ┆ null │
│ 2   ┆ ham  │
│ 3   ┆ spam │
└─────┴──────┘
```
To see how this works we can take a look at the query plan. Below we see that all files are read
separately and concatenated into a single `DataFrame`. Polars will try to parallelize the reading.

```
pl.scan_csv("docs/assets/data/my_many_files_*.csv").show_graph()
```
## Reading and processing in parallel

If your files don't have to be in a single table you can also build a query plan for each file and execute them in parallel on the Polars thread pool.

All query plan execution is embarrassingly parallel and doesn't require any communication.

```
import glob
import polars as pl
queries = []
for file in glob.glob("docs/assets/data/my_many_files_*.csv"):
    q = pl.scan_csv(file).group_by("bar").agg(pl.len(), pl.sum("foo"))
    queries.append(q)
dataframes = pl.collect_all(queries)
print(dataframes)
```
```
[shape: (3, 3)
┌──────┬─────┬─────┐
│ bar  ┆ len ┆ foo │
│ ---  ┆ --- ┆ --- │
│ str  ┆ u32 ┆ i64 │
╞══════╪═════╪═════╡
│ spam ┆ 1   ┆ 3   │
│ ham  ┆ 1   ┆ 2   │
│ null ┆ 1   ┆ 1   │
└──────┴─────┴─────┘, shape: (3, 3)
┌──────┬─────┬─────┐
│ bar  ┆ len ┆ foo │
│ ---  ┆ --- ┆ --- │
│ str  ┆ u32 ┆ i64 │
╞══════╪═════╪═════╡
│ null ┆ 1   ┆ 1   │
│ ham  ┆ 1   ┆ 2   │
│ spam ┆ 1   ┆ 3   │
└──────┴─────┴─────┘, shape: (3, 3)
┌──────┬─────┬─────┐
│ bar  ┆ len ┆ foo │
│ ---  ┆ --- ┆ --- │
│ str  ┆ u32 ┆ i64 │
╞══════╪═════╪═════╡
│ ham  ┆ 1   ┆ 2   │
│ null ┆ 1   ┆ 1   │
│ spam ┆ 1   ┆ 3   │
└──────┴─────┴─────┘, shape: (3, 3)
┌──────┬─────┬─────┐
│ bar  ┆ len ┆ foo │
│ ---  ┆ --- ┆ --- │
│ str  ┆ u32 ┆ i64 │
╞══════╪═════╪═════╡
│ spam ┆ 1   ┆ 3   │
│ ham  ┆ 1   ┆ 2   │
│ null ┆ 1   ┆ 1   │
└──────┴─────┴─────┘, shape: (3, 3)
┌──────┬─────┬─────┐
│ bar  ┆ len ┆ foo │
│ ---  ┆ --- ┆ --- │
│ str  ┆ u32 ┆ i64 │
╞══════╪═════╪═════╡
│ spam ┆ 1   ┆ 3   │
│ ham  ┆ 1   ┆ 2   │
│ null ┆ 1   ┆ 1   │
└──────┴─────┴─────┘]
```

# Citations

1. Source page: https://docs.pola.rs/user-guide/io/multiple
