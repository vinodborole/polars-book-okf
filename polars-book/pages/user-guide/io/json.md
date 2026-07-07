---
type: Web Page
title: JSON files - Polars user guide
resource: https://docs.pola.rs/user-guide/io/json
timestamp: '2026-07-07T12:26:19.464100+00:00'
---

# JSON files

Polars can read and write both standard JSON and newline-delimited JSON (NDJSON).

## Read

### JSON

Reading a JSON file should look familiar:

```
df = pl.read_json("docs/assets/data/path.json")
```
  `JsonReader` ·  Available on feature json

```
use polars::prelude::*;
let mut file = std::fs::File::open("docs/assets/data/path.json").unwrap();
let df = JsonReader::new(&mut file).finish()?;
```
### Newline Delimited JSON

JSON objects that are delimited by newlines can be read into Polars in a much more performant way than standard json.

Polars can read an NDJSON file into a `DataFrame` using the `read_ndjson` function:

```
df = pl.read_ndjson("docs/assets/data/path.json")
```
  `JsonReader` ·  Available on feature json

```
let df = LazyJsonLineReader::new(PlRefPath::new("docs/assets/data/path.json"))
    .finish()
    .unwrap()
    .collect()
    .unwrap();
```
## Write

```
df = pl.DataFrame({"foo": [1, 2, 3], "bar": [None, "bak", "baz"]})
df.write_json("docs/assets/data/path.json")
```
  `JsonWriter` ·  `JsonWriter` ·  Available on feature json

```
let mut df = df!(
    "foo" => &[1, 2, 3],
    "bar" => &[None, Some("bak"), Some("baz")],
)
.unwrap();
let mut file = std::fs::File::create("docs/assets/data/path.json").unwrap();
// json
JsonWriter::new(&mut file)
    .with_json_format(JsonFormat::Json)
    .finish(&mut df)
    .unwrap();
// ndjson
JsonWriter::new(&mut file)
    .with_json_format(JsonFormat::JsonLines)
    .finish(&mut df)
    .unwrap();
```
## Scan

Polars allows you to *scan* a NDJSON input. Scanning delays the actual parsing of the file and
instead returns a lazy computation holder called a `LazyFrame`.

```
df = pl.scan_ndjson("docs/assets/data/path.json")
```
  `LazyJsonLineReader` ·  Available on feature json

```
let lf = LazyJsonLineReader::new(PlRefPath::new("docs/assets/data/path.json"))
    .finish()
    .unwrap();
```

# Citations

1. Source page: https://docs.pola.rs/user-guide/io/json
