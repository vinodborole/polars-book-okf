---
type: Web Page
title: Hugging Face - Polars user guide
resource: https://docs.pola.rs/user-guide/io/hugging-face
timestamp: '2026-08-03T09:49:29.273788+00:00'
---

# Hugging Face

## Scanning datasets from Hugging Face

All cloud-enabled scan functions, and their `read_` counterparts transparently support scanning from
Hugging Face:

| Scan | Read | 
|---|---|
| [scan_parquet](https://docs.pola.rs/api/python/stable/reference/api/polars.scan_parquet.html) | [read_parquet](https://docs.pola.rs/api/python/stable/reference/api/polars.read_parquet.html) | 
| [scan_csv](https://docs.pola.rs/api/python/stable/reference/api/polars.scan_csv.html) | [read_csv](https://docs.pola.rs/api/python/stable/reference/api/polars.read_csv.html) | 
| [scan_ndjson](https://docs.pola.rs/api/python/stable/reference/api/polars.scan_ndjson.html) | [read_ndjson](https://docs.pola.rs/api/python/stable/reference/api/polars.read_ndjson.html) | 
| [scan_ipc](https://docs.pola.rs/api/python/stable/reference/api/polars.scan_ipc.html) | [read_ipc](https://docs.pola.rs/api/python/stable/reference/api/polars.read_ipc.html) | 

### Path format

To scan from Hugging Face, a `hf://` path can be passed to the scan functions. The `hf://` path
format is defined as `hf://BUCKET/REPOSITORY@REVISION/PATH`, where:

- `BUCKET` is one of`datasets` or`spaces`
- `REPOSITORY` is the location of the repository, this is usually in the format of`username/repo_name` . A branch can also be optionally specified by appending`@branch`
- `REVISION` is the name of the branch (or commit) to use. This is optional and defaults to`main` if not given.
- `PATH` is a file or directory path, or a glob pattern from the repository root.

Example `hf://` paths:

| Path | Path components | 
|---|---|
| hf://datasets/nameexhaustion/polars-docs/iris.csv | Bucket: datasets Repository: nameexhaustion/polars-docs Branch: main Path: iris.csv [Web URL](https://huggingface.co/datasets/nameexhaustion/polars-docs/tree/main/) | 
| hf://datasets/nameexhaustion/polars-docs@foods/*.csv | Bucket: datasets Repository: nameexhaustion/polars-docs Branch: foods Path: *.csv [Web URL](https://huggingface.co/datasets/nameexhaustion/polars-docs/tree/foods/) | 
| hf://datasets/nameexhaustion/polars-docs/hive_dates/ | Bucket: datasets Repository: nameexhaustion/polars-docs Branch: main Path: hive_dates/ [Web URL](https://huggingface.co/datasets/nameexhaustion/polars-docs/tree/main/hive_dates/) | 
| hf://spaces/nameexhaustion/polars-docs/orders.feather | Bucket: spaces Repository: nameexhaustion/polars-docs Branch: main Path: orders.feather [Web URL](https://huggingface.co/spaces/nameexhaustion/polars-docs/tree/main/) | 

### Authentication

A Hugging Face API key can be passed to Polars to access private locations using either of the following methods:

- Passing a `token` in`storage_options` to the scan function, e.g.`scan_parquet(..., storage_options={'token': '<your HF token>'})`
- Setting the `HF_TOKEN` environment variable, e.g.`export HF_TOKEN=<your HF token>`

### Examples

#### CSV

```
print(pl.scan_csv("hf://datasets/nameexhaustion/polars-docs/iris.csv").collect())
```
```
shape: (150, 5)
┌──────────────┬─────────────┬──────────────┬─────────────┬───────────┐
│ sepal_length ┆ sepal_width ┆ petal_length ┆ petal_width ┆ species   │
│ ---          ┆ ---         ┆ ---          ┆ ---         ┆ ---       │
│ f64          ┆ f64         ┆ f64          ┆ f64         ┆ str       │
╞══════════════╪═════════════╪══════════════╪═════════════╪═══════════╡
│ 5.1          ┆ 3.5         ┆ 1.4          ┆ 0.2         ┆ setosa    │
│ 4.9          ┆ 3.0         ┆ 1.4          ┆ 0.2         ┆ setosa    │
│ 4.7          ┆ 3.2         ┆ 1.3          ┆ 0.2         ┆ setosa    │
│ 4.6          ┆ 3.1         ┆ 1.5          ┆ 0.2         ┆ setosa    │
│ 5.0          ┆ 3.6         ┆ 1.4          ┆ 0.2         ┆ setosa    │
│ …            ┆ …           ┆ …            ┆ …           ┆ …         │
│ 6.7          ┆ 3.0         ┆ 5.2          ┆ 2.3         ┆ virginica │
│ 6.3          ┆ 2.5         ┆ 5.0          ┆ 1.9         ┆ virginica │
│ 6.5          ┆ 3.0         ┆ 5.2          ┆ 2.0         ┆ virginica │
│ 6.2          ┆ 3.4         ┆ 5.4          ┆ 2.3         ┆ virginica │
│ 5.9          ┆ 3.0         ┆ 5.1          ┆ 1.8         ┆ virginica │
└──────────────┴─────────────┴──────────────┴─────────────┴───────────┘
```
See this file at
[https://huggingface.co/datasets/nameexhaustion/polars-docs/blob/main/iris.csv](https://huggingface.co/datasets/nameexhaustion/polars-docs/blob/main/iris.csv)

#### NDJSON

```
print(pl.scan_ndjson("hf://datasets/nameexhaustion/polars-docs/iris.jsonl").collect())
```
```
shape: (150, 5)
┌──────────────┬─────────────┬──────────────┬─────────────┬───────────┐
│ sepal_length ┆ sepal_width ┆ petal_length ┆ petal_width ┆ species   │
│ ---          ┆ ---         ┆ ---          ┆ ---         ┆ ---       │
│ f64          ┆ f64         ┆ f64          ┆ f64         ┆ str       │
╞══════════════╪═════════════╪══════════════╪═════════════╪═══════════╡
│ 5.1          ┆ 3.5         ┆ 1.4          ┆ 0.2         ┆ setosa    │
│ 4.9          ┆ 3.0         ┆ 1.4          ┆ 0.2         ┆ setosa    │
│ 4.7          ┆ 3.2         ┆ 1.3          ┆ 0.2         ┆ setosa    │
│ 4.6          ┆ 3.1         ┆ 1.5          ┆ 0.2         ┆ setosa    │
│ 5.0          ┆ 3.6         ┆ 1.4          ┆ 0.2         ┆ setosa    │
│ …            ┆ …           ┆ …            ┆ …           ┆ …         │
│ 6.7          ┆ 3.0         ┆ 5.2          ┆ 2.3         ┆ virginica │
│ 6.3          ┆ 2.5         ┆ 5.0          ┆ 1.9         ┆ virginica │
│ 6.5          ┆ 3.0         ┆ 5.2          ┆ 2.0         ┆ virginica │
│ 6.2          ┆ 3.4         ┆ 5.4          ┆ 2.3         ┆ virginica │
│ 5.9          ┆ 3.0         ┆ 5.1          ┆ 1.8         ┆ virginica │
└──────────────┴─────────────┴──────────────┴─────────────┴───────────┘
```
See this file at
[https://huggingface.co/datasets/nameexhaustion/polars-docs/blob/main/iris.jsonl](https://huggingface.co/datasets/nameexhaustion/polars-docs/blob/main/iris.jsonl)

#### Parquet

```
print(
    """\
shape: (4, 3)
┌────────────┬────────────────────────────┬─────┐
│ date1      ┆ date2                      ┆ x   │
│ ---        ┆ ---                        ┆ --- │
│ date       ┆ datetime[μs]               ┆ i32 │
╞════════════╪════════════════════════════╪═════╡
│ 2024-01-01 ┆ 2023-01-01 00:00:00        ┆ 1   │
│ 2024-02-01 ┆ 2023-02-01 00:00:00        ┆ 2   │
│ 2024-03-01 ┆ null                       ┆ 3   │
│ null       ┆ 2023-03-01 01:01:01.000001 ┆ 4   │
└────────────┴────────────────────────────┴─────┘
"""
)
```
```
shape: (4, 3)
┌────────────┬────────────────────────────┬─────┐
│ date1      ┆ date2                      ┆ x   │
│ ---        ┆ ---                        ┆ --- │
│ date       ┆ datetime[μs]               ┆ i32 │
╞════════════╪════════════════════════════╪═════╡
│ 2024-01-01 ┆ 2023-01-01 00:00:00        ┆ 1   │
│ 2024-02-01 ┆ 2023-02-01 00:00:00        ┆ 2   │
│ 2024-03-01 ┆ null                       ┆ 3   │
│ null       ┆ 2023-03-01 01:01:01.000001 ┆ 4   │
└────────────┴────────────────────────────┴─────┘
```
See this folder at
[https://huggingface.co/datasets/nameexhaustion/polars-docs/tree/main/hive_dates/](https://huggingface.co/datasets/nameexhaustion/polars-docs/tree/main/hive_dates/)

#### IPC

```
print(pl.scan_ipc("hf://spaces/nameexhaustion/polars-docs/orders.feather").collect())
```
```
shape: (10, 9)
┌────────────┬───────────┬───────────────┬──────────────┬───┬─────────────────┬─────────────────┬────────────────┬─────────────────────────┐
│ o_orderkey ┆ o_custkey ┆ o_orderstatus ┆ o_totalprice ┆ … ┆ o_orderpriority ┆ o_clerk         ┆ o_shippriority ┆ o_comment               │
│ ---        ┆ ---       ┆ ---           ┆ ---          ┆   ┆ ---             ┆ ---             ┆ ---            ┆ ---                     │
│ i64        ┆ i64       ┆ str           ┆ f64          ┆   ┆ str             ┆ str             ┆ i64            ┆ str                     │
╞════════════╪═══════════╪═══════════════╪══════════════╪═══╪═════════════════╪═════════════════╪════════════════╪═════════════════════════╡
│ 1          ┆ 36901     ┆ O             ┆ 173665.47    ┆ … ┆ 5-LOW           ┆ Clerk#000000951 ┆ 0              ┆ nstructions sleep       │
│            ┆           ┆               ┆              ┆   ┆                 ┆                 ┆                ┆ furiously am…           │
│ 2          ┆ 78002     ┆ O             ┆ 46929.18     ┆ … ┆ 1-URGENT        ┆ Clerk#000000880 ┆ 0              ┆ foxes. pending accounts │
│            ┆           ┆               ┆              ┆   ┆                 ┆                 ┆                ┆ at th…                  │
│ 3          ┆ 123314    ┆ F             ┆ 193846.25    ┆ … ┆ 5-LOW           ┆ Clerk#000000955 ┆ 0              ┆ sly final accounts      │
│            ┆           ┆               ┆              ┆   ┆                 ┆                 ┆                ┆ boost. care…            │
│ 4          ┆ 136777    ┆ O             ┆ 32151.78     ┆ … ┆ 5-LOW           ┆ Clerk#000000124 ┆ 0              ┆ sits. slyly regular     │
│            ┆           ┆               ┆              ┆   ┆                 ┆                 ┆                ┆ warthogs c…             │
│ 5          ┆ 44485     ┆ F             ┆ 144659.2     ┆ … ┆ 5-LOW           ┆ Clerk#000000925 ┆ 0              ┆ quickly. bold deposits  │
│            ┆           ┆               ┆              ┆   ┆                 ┆                 ┆                ┆ sleep s…                │
│ 6          ┆ 55624     ┆ F             ┆ 58749.59     ┆ … ┆ 4-NOT SPECIFIED ┆ Clerk#000000058 ┆ 0              ┆ ggle. special, final    │
│            ┆           ┆               ┆              ┆   ┆                 ┆                 ┆                ┆ requests …              │
│ 7          ┆ 39136     ┆ O             ┆ 252004.18    ┆ … ┆ 2-HIGH          ┆ Clerk#000000470 ┆ 0              ┆ ly special requests     │
│ 32         ┆ 130057    ┆ O             ┆ 208660.75    ┆ … ┆ 2-HIGH          ┆ Clerk#000000616 ┆ 0              ┆ ise blithely bold,      │
│            ┆           ┆               ┆              ┆   ┆                 ┆                 ┆                ┆ regular req…            │
│ 33         ┆ 66958     ┆ F             ┆ 163243.98    ┆ … ┆ 3-MEDIUM        ┆ Clerk#000000409 ┆ 0              ┆ uriously. furiously     │
│            ┆           ┆               ┆              ┆   ┆                 ┆                 ┆                ┆ final requ…             │
│ 34         ┆ 61001     ┆ O             ┆ 58949.67     ┆ … ┆ 3-MEDIUM        ┆ Clerk#000000223 ┆ 0              ┆ ly final packages.      │
│            ┆           ┆               ┆              ┆   ┆                 ┆                 ┆                ┆ fluffily fi…            │
└────────────┴───────────┴───────────────┴──────────────┴───┴─────────────────┴─────────────────┴────────────────┴─────────────────────────┘
```
See this file at
[https://huggingface.co/spaces/nameexhaustion/polars-docs/blob/main/orders.feather](https://huggingface.co/spaces/nameexhaustion/polars-docs/blob/main/orders.feather)

# Citations

1. Source page: https://docs.pola.rs/user-guide/io/hugging-face
