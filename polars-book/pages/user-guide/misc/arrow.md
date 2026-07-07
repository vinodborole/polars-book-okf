---
type: Web Page
title: Arrow producer/consumer - Polars user guide
resource: https://docs.pola.rs/user-guide/misc/arrow
timestamp: '2026-07-07T12:26:19.464100+00:00'
---

# Arrow producer/consumer

## Using pyarrow

Polars can move data in and out of arrow zero copy. This can be done either via pyarrow or natively. Let's first start by showing the pyarrow solution:

```
import polars as pl
df = pl.DataFrame({"foo": [1, 2, 3], "bar": ["ham", "spam", "jam"]})
arrow_table = df.to_arrow()
print(arrow_table)
```
```
pyarrow.Table
foo: int64
bar: large_string
----
foo: [[1,2,3]]
bar: [["ham","spam","jam"]]
```
Or if you want to ensure the output is zero-copy:

```
arrow_table_zero_copy = df.to_arrow(compat_level=pl.CompatLevel.newest())
print(arrow_table_zero_copy)
```
```
pyarrow.Table
foo: int64
bar: string_view
----
foo: [[1,2,3]]
bar: [["ham","spam","jam"]]
```
Importing from pyarrow can be achieved with `pl.from_arrow`.

## Using the Arrow PyCapsule Interface

As of Polars v1.3 and higher, Polars implements the Arrow PyCapsule Interface, a protocol for sharing Arrow data across Python libraries.

### Exporting data from Polars to pyarrow

To convert a Polars `DataFrame` to a `pyarrow.Table`, use the `pyarrow.table` constructor:

Note

This requires pyarrow v15 or higher.

```
import polars as pl
import pyarrow as pa
df = pl.DataFrame({"foo": [1, 2, 3], "bar": ["ham", "spam", "jam"]})
arrow_table = pa.table(df)
print(arrow_table)
```
```
pyarrow.Table
foo: int64
bar: string_view
----
foo: [[1,2,3]]
bar: [["ham","spam","jam"]]
```
To convert a Polars `Series` to a `pyarrow.ChunkedArray`, use the `pyarrow.chunked_array`
constructor.

```
arrow_chunked_array = pa.chunked_array(df["foo"])
print(arrow_chunked_array)
```
```
[
  [
    1,
    2,
    3
  ]
]
```
You can also pass a `Series` to the `pyarrow.array` constructor to create a contiguous array. Note
that this will not be zero-copy if the underlying `Series` had multiple chunks.

```
arrow_array = pa.array(df["foo"])
print(arrow_array)
```
```
[
  1,
  2,
  3
]
```
### Importing data from pyarrow to Polars

We can pass the pyarrow `Table` back to Polars by using the `polars.DataFrame` constructor:

```
polars_df = pl.DataFrame(arrow_table)
print(polars_df)
```
```
shape: (3, 2)
┌─────┬──────┐
│ foo ┆ bar  │
│ --- ┆ ---  │
│ i64 ┆ str  │
╞═════╪══════╡
│ 1   ┆ ham  │
│ 2   ┆ spam │
│ 3   ┆ jam  │
└─────┴──────┘
```
Similarly, we can pass the pyarrow `ChunkedArray` or `Array` back to Polars by using the
`polars.Series` constructor:

```
polars_series = pl.Series(arrow_chunked_array)
print(polars_series)
```
```
shape: (3,)
Series: '' [i64]
[
    1
    2
    3
]
```
### Usage with other arrow libraries

There's a growing list of
libraries that support the PyCapsule Interface directly. Polars `Series` and `DataFrame` objects
work automatically with every such library.

### For library maintainers

If you're developing a library that you wish to integrate with Polars, it's suggested to implement the Arrow PyCapsule Interface yourself. This comes with a number of benefits:

- Zero-copy exchange for both Polars Series and DataFrame
- No required dependency on pyarrow.
- No direct dependency on Polars.
- Harder to cause memory leaks than handling pointers as raw integers.
- Automatic zero-copy integration other PyCapsule Interface-supported libraries.

## Using Polars directly

Polars can also consume and export to and import from the Arrow C Data Interface directly. This is recommended for libraries that don't support the Arrow PyCapsule Interface and want to interop with Polars without requiring a pyarrow installation.

- To export `ArrowArray`C structs, Polars exposes:`Series._export_arrow_to_c`.
- To import an `ArrowArray`C struct, Polars exposes`Series._import_arrow_from_c`.

# Citations

1. Source page: https://docs.pola.rs/user-guide/misc/arrow
