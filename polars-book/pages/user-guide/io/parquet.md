---
type: Web Page
title: Parquet - Polars user guide
resource: https://docs.pola.rs/user-guide/io/parquet
timestamp: '2026-07-07T12:26:19.464100+00:00'
---

# Parquet

Loading or writing `Parquet` files is lightning fast as the layout of
data in a Polars `DataFrame` in memory mirrors the layout of a Parquet file on disk in many
respects.

Unlike CSV, Parquet is a columnar format. This means that the data is stored in columns rather than rows. This is a more efficient way of storing data as it allows for better compression and faster access to data.

## Read

We can read a `Parquet` file into a `DataFrame` using the `read_parquet` function:

```
df = pl.read_parquet("docs/assets/data/path.parquet")
```
  `ParquetReader` ·  Available on feature parquet

```
let mut file = std::fs::File::open("docs/assets/data/path.parquet").unwrap();
let df = ParquetReader::new(&mut file).finish().unwrap();
```
## Write

```
df = pl.DataFrame({"foo": [1, 2, 3], "bar": [None, "bak", "baz"]})
df.write_parquet("docs/assets/data/path.parquet")
```
  `ParquetWriter` ·  Available on feature parquet

```
let mut df = df!(
    "foo" => &[1, 2, 3],
    "bar" => &[None, Some("bak"), Some("baz")],
)
.unwrap();
let mut file = std::fs::File::create("docs/assets/data/path.parquet").unwrap();
ParquetWriter::new(&mut file).finish(&mut df).unwrap();
```
## Scan

Polars allows you to *scan* a `Parquet` input. Scanning delays the actual parsing of the file and
instead returns a lazy computation holder called a `LazyFrame`.

```
df = pl.scan_parquet("docs/assets/data/path.parquet")
```
  `scan_parquet` ·  Available on feature parquet

```
let args = ScanArgsParquet::default();
let lf =
    LazyFrame::scan_parquet(PlRefPath::new("docs/assets/data/path.parquet"), args).unwrap();
```
If you want to know why this is desirable, you can read more about those Polars optimizations here.

When we scan a `Parquet` file stored in the cloud, we can also apply predicate and projection
pushdowns. This can significantly reduce the amount of data that needs to be downloaded. For
scanning a Parquet file in the cloud, see
Cloud storage.

# Citations

1. Source page: https://docs.pola.rs/user-guide/io/parquet
