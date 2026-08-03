---
type: Web Page
title: Hive - Polars user guide
resource: https://docs.pola.rs/user-guide/io/hive
timestamp: '2026-08-03T09:49:29.273788+00:00'
---

# Hive

## Scanning hive partitioned data

Polars supports scanning hive partitioned parquet and IPC datasets, with planned support for other formats in the future.

Hive partition parsing is enabled by default if `scan_parquet` receives a single directory path,
otherwise it is disabled by default. This can be explicitly configured using the `hive_partitioning`
parameter.

### Scanning a hive directory

For this example the following directory structure is used:

```
┌───────────────────────────────────────────────────────┐
│ File path                                             │
╞═══════════════════════════════════════════════════════╡
│ docs/assets/data/hive/year=2023/month=11/data.parquet │
│ docs/assets/data/hive/year=2023/month=12/data.parquet │
│ docs/assets/data/hive/year=2024/month=01/data.parquet │
│ docs/assets/data/hive/year=2024/month=02/data.parquet │
└───────────────────────────────────────────────────────┘
```
Simply pass the directory to `scan_parquet`, and all files will be loaded with the hive parts in the
path included in the output:

```
import polars as pl
df = pl.scan_parquet("docs/assets/data/hive/").collect()
with pl.Config(tbl_rows=99):
    print(df)
```
```
shape: (11, 3)
┌─────┬──────┬───────┐
│ x   ┆ year ┆ month │
│ --- ┆ ---  ┆ ---   │
│ i64 ┆ i64  ┆ i64   │
╞═════╪══════╪═══════╡
│ 1   ┆ 2023 ┆ 11    │
│ 2   ┆ 2023 ┆ 11    │
│ 3   ┆ 2023 ┆ 12    │
│ 4   ┆ 2023 ┆ 12    │
│ 5   ┆ 2023 ┆ 12    │
│ 6   ┆ 2024 ┆ 1     │
│ 7   ┆ 2024 ┆ 1     │
│ 8   ┆ 2024 ┆ 2     │
│ 9   ┆ 2024 ┆ 2     │
│ 10  ┆ 2024 ┆ 2     │
│ 11  ┆ 2024 ┆ 2     │
└─────┴──────┴───────┘
```
### Handling mixed files

Passing a directory to `scan_parquet` may not work if there are files with different extensions in
the directory.

For this example the following directory structure is used:

```
┌─────────────────────────────────────────────────────────────┐
│ File path                                                   │
╞═════════════════════════════════════════════════════════════╡
│ docs/assets/data/hive_mixed/description.txt                 │
│ docs/assets/data/hive_mixed/year=2023/month=11/data.parquet │
│ docs/assets/data/hive_mixed/year=2023/month=12/data.parquet │
│ docs/assets/data/hive_mixed/year=2024/month=01/data.parquet │
│ docs/assets/data/hive_mixed/year=2024/month=02/data.parquet │
└─────────────────────────────────────────────────────────────┘
```
```
from pathlib import Path
try:
    pl.scan_parquet("docs/assets/data/hive_mixed/").collect()
except Exception as e:
    print(e)
```
The above fails as `description.txt` is not a valid parquet file:

```
directory contained paths with different file extensions: first path: docs/assets/data/hive_mixed/description.txt, second path: docs/assets/data/hive_mixed/year=2024/month=02/data.parquet. Please use a glob pattern to explicitly specify which files to read (e.g. 'dir/**/*', 'dir/**/*.parquet')
```
In this situation, a glob pattern can be used to be more specific about which files to load. Note
that `hive_partitioning` must explicitly set to `True`:

```
df = pl.scan_parquet(
    # Glob to match all files ending in `.parquet`
    "docs/assets/data/hive_mixed/**/*.parquet",
    hive_partitioning=True,
).collect()
with pl.Config(tbl_rows=99):
    print(df)
```
```
shape: (11, 3)
┌─────┬──────┬───────┐
│ x   ┆ year ┆ month │
│ --- ┆ ---  ┆ ---   │
│ i64 ┆ i64  ┆ i64   │
╞═════╪══════╪═══════╡
│ 1   ┆ 2023 ┆ 11    │
│ 2   ┆ 2023 ┆ 11    │
│ 3   ┆ 2023 ┆ 12    │
│ 4   ┆ 2023 ┆ 12    │
│ 5   ┆ 2023 ┆ 12    │
│ 6   ┆ 2024 ┆ 1     │
│ 7   ┆ 2024 ┆ 1     │
│ 8   ┆ 2024 ┆ 2     │
│ 9   ┆ 2024 ┆ 2     │
│ 10  ┆ 2024 ┆ 2     │
│ 11  ┆ 2024 ┆ 2     │
└─────┴──────┴───────┘
```
### Scanning file paths with hive parts

`hive_partitioning` is not enabled by default for file paths:

```
df = pl.scan_parquet(
    [
        "docs/assets/data/hive/year=2024/month=01/data.parquet",
        "docs/assets/data/hive/year=2024/month=02/data.parquet",
    ],
).collect()
print(df)
```
```
shape: (6, 1)
┌─────┐
│ x   │
│ --- │
│ i64 │
╞═════╡
│ 6   │
│ 7   │
│ 8   │
│ 9   │
│ 10  │
│ 11  │
└─────┘
```
Pass `hive_partitioning=True` to enable hive partition parsing:

```
df = pl.scan_parquet(
    [
        "docs/assets/data/hive/year=2024/month=01/data.parquet",
        "docs/assets/data/hive/year=2024/month=02/data.parquet",
    ],
    hive_partitioning=True,
).collect()
print(df)
```
```
shape: (6, 3)
┌─────┬──────┬───────┐
│ x   ┆ year ┆ month │
│ --- ┆ ---  ┆ ---   │
│ i64 ┆ i64  ┆ i64   │
╞═════╪══════╪═══════╡
│ 6   ┆ 2024 ┆ 1     │
│ 7   ┆ 2024 ┆ 1     │
│ 8   ┆ 2024 ┆ 2     │
│ 9   ┆ 2024 ┆ 2     │
│ 10  ┆ 2024 ┆ 2     │
│ 11  ┆ 2024 ┆ 2     │
└─────┴──────┴───────┘
```
## Writing hive partitioned data

Note: The following functionality is considered *unstable*, and is subject to change.

Polars supports writing hive partitioned parquet datasets, with planned support for other formats.

### Example

For this example the following DataFrame is used:

```
df = pl.DataFrame({"a": [1, 1, 2, 2, 3], "b": [1, 1, 1, 2, 2], "c": 1})
print(df)
```
```
shape: (5, 3)
┌─────┬─────┬─────┐
│ a   ┆ b   ┆ c   │
│ --- ┆ --- ┆ --- │
│ i64 ┆ i64 ┆ i32 │
╞═════╪═════╪═════╡
│ 1   ┆ 1   ┆ 1   │
│ 1   ┆ 1   ┆ 1   │
│ 2   ┆ 1   ┆ 1   │
│ 2   ┆ 2   ┆ 1   │
│ 3   ┆ 2   ┆ 1   │
└─────┴─────┴─────┘
```
We will write it to a hive-partitioned parquet dataset, partitioned by the columns `a` and `b`:

```
df.write_parquet("docs/assets/data/hive_write/", partition_by=["a", "b"])
```
The output is a hive partitioned parquet dataset with the following paths:

```
┌──────────────────────────────────────────────────────┐
│ File path                                            │
╞══════════════════════════════════════════════════════╡
│ docs/assets/data/hive_write/a=1/b=1/00000000.parquet │
│ docs/assets/data/hive_write/a=2/b=1/00000000.parquet │
│ docs/assets/data/hive_write/a=2/b=2/00000000.parquet │
│ docs/assets/data/hive_write/a=3/b=2/00000000.parquet │
└──────────────────────────────────────────────────────┘
```

# Citations

1. Source page: https://docs.pola.rs/user-guide/io/hive
