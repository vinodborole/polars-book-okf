---
type: Web Page
title: CSV - Polars user guide
resource: https://docs.pola.rs/user-guide/io/csv
timestamp: '2026-07-07T12:26:19.464100+00:00'
---

# CSV

## Read & write

Reading a CSV file should look familiar:

```
df = pl.read_csv("docs/assets/data/path.csv")
```
  `CsvReader` ·  Available on feature csv

```
use polars::prelude::*;
let mut df = df!(
    "foo" => &[1, 2, 3],
    "bar" => &[None, Some("bak"), Some("baz")],
)
.unwrap();
let mut file = std::fs::File::create("docs/assets/data/path.csv").unwrap();
CsvWriter::new(&mut file).finish(&mut df).unwrap();
let df = CsvReadOptions::default()
    .try_into_reader_with_file_path(Some("docs/assets/data/path.csv".into()))
    .unwrap()
    .finish()
    .unwrap();
```
Writing a CSV file is similar with the `write_csv` function:

```
df = pl.DataFrame({"foo": [1, 2, 3], "bar": [None, "bak", "baz"]})
df.write_csv("docs/assets/data/path.csv")
```
  `CsvWriter` ·  Available on feature csv

```
let mut df = df!(
    "foo" => &[1, 2, 3],
    "bar" => &[None, Some("bak"), Some("baz")],
)
.unwrap();
let mut file = std::fs::File::create("docs/assets/data/path.csv").unwrap();
CsvWriter::new(&mut file).finish(&mut df).unwrap();
```
## Scan

Polars allows you to *scan* a CSV input. Scanning delays the actual parsing of the file and instead
returns a lazy computation holder called a `LazyFrame`.

```
df = pl.scan_csv("docs/assets/data/path.csv")
```
  `LazyCsvReader` ·  Available on feature csv

```
let lf = LazyCsvReader::new(PlRefPath::new("docs/assets/data/path.csv"))
    .finish()
    .unwrap();
```
If you want to know why this is desirable, you can read more about these Polars optimizations here.

# Citations

1. Source page: https://docs.pola.rs/user-guide/io/csv
