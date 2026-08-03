---
type: Web Page
title: Parsing - Polars user guide
resource: https://docs.pola.rs/user-guide/transformations/time-series/parsing
timestamp: '2026-08-03T09:49:29.273788+00:00'
---

# Parsing

Polars has native support for parsing time series data and doing more sophisticated operations such as temporal grouping and resampling.

## Datatypes

Polars has the following datetime datatypes:

- `Date` : Date representation e.g. 2014-07-08. It is internally represented as days since UNIX epoch
  encoded by a 32-bit signed integer.
- `Datetime` : Datetime representation e.g. 2014-07-08 07:00:00. It is internally represented as a 64
  bit integer since the Unix epoch and can have different units such as ns, us, ms.
- `Duration` : A time delta type that is created when subtracting`Date/Datetime` . Similar to`timedelta` in Python.
- `Time` : Time representation, internally represented as nanoseconds since midnight.

## Parsing dates from a file

When loading from a CSV file Polars attempts to parse dates and times if the `try_parse_dates` flag
is set to `True`:

```
df = pl.read_csv("docs/assets/data/apple_stock.csv", try_parse_dates=True)
print(df)
```
  [`CsvReader`](https://docs.pola.rs/api/rust/dev/polars/prelude/struct.CsvReader.html) ·  [Available on feature csv](/user-guide/installation/#feature-flags)

```
let df = CsvReadOptions::default()
    .map_parse_options(|parse_options| parse_options.with_try_parse_dates(true))
    .try_into_reader_with_file_path(Some("docs/assets/data/apple_stock.csv".into()))
    .unwrap()
    .finish()
    .unwrap();
println!("{}", &df);
```
```
shape: (100, 2)
┌────────────┬────────┐
│ Date       ┆ Close  │
│ ---        ┆ ---    │
│ date       ┆ f64    │
╞════════════╪════════╡
│ 1981-02-23 ┆ 24.62  │
│ 1981-05-06 ┆ 27.38  │
│ 1981-05-18 ┆ 28.0   │
│ 1981-09-25 ┆ 14.25  │
│ 1982-07-08 ┆ 11.0   │
│ …          ┆ …      │
│ 2012-05-16 ┆ 546.08 │
│ 2012-12-04 ┆ 575.85 │
│ 2013-07-05 ┆ 417.42 │
│ 2013-11-07 ┆ 512.49 │
│ 2014-02-25 ┆ 522.06 │
└────────────┴────────┘
```
This flag will trigger schema inference on a number of rows, as configured by the
`infer_schema_length` setting (100 rows by default). Schema inference is computationally expensive
and can slow down file loading if a high number of rows is used.

On the other hand binary formats such as parquet have a schema that is respected by Polars.

## Casting strings to dates

You can also cast a column of datetimes encoded as strings to a datetime type. You do this by
calling the string `str.to_date` method and passing the format of the date string:

```
df = pl.read_csv("docs/assets/data/apple_stock.csv", try_parse_dates=False)
df = df.with_columns(pl.col("Date").str.to_date("%Y-%m-%d"))
print(df)
```
  [`CsvReader`](https://docs.pola.rs/api/rust/dev/polars/prelude/struct.CsvReader.html) ·  [`str.replace_all`](https://docs.pola.rs/api/rust/dev/polars_lazy/dsl/string/struct.StringNameSpace.html#method.to_date) ·  [Available on feature csv](/user-guide/installation/#feature-flags) ·  [Available on feature dtype-date](/user-guide/installation/#feature-flags)

```
let df = CsvReadOptions::default()
    .map_parse_options(|parse_options| parse_options.with_try_parse_dates(false))
    .try_into_reader_with_file_path(Some("docs/assets/data/apple_stock.csv".into()))
    .unwrap()
    .finish()
    .unwrap();
let df = df
    .lazy()
    .with_columns([col("Date").str().to_date(StrptimeOptions::default())])
    .collect()?;
println!("{}", &df);
```
```
shape: (100, 2)
┌────────────┬────────┐
│ Date       ┆ Close  │
│ ---        ┆ ---    │
│ date       ┆ f64    │
╞════════════╪════════╡
│ 1981-02-23 ┆ 24.62  │
│ 1981-05-06 ┆ 27.38  │
│ 1981-05-18 ┆ 28.0   │
│ 1981-09-25 ┆ 14.25  │
│ 1982-07-08 ┆ 11.0   │
│ …          ┆ …      │
│ 2012-05-16 ┆ 546.08 │
│ 2012-12-04 ┆ 575.85 │
│ 2013-07-05 ┆ 417.42 │
│ 2013-11-07 ┆ 512.49 │
│ 2014-02-25 ┆ 522.06 │
└────────────┴────────┘
```
[The format string specification can be found here.](https://docs.rs/chrono/latest/chrono/format/strftime/index.html).

## Extracting date features from a date column

You can extract data features such as the year or day from a date column using the `.dt` namespace:

```
shape: (100, 3)
┌────────────┬────────┬──────┐
│ Date       ┆ Close  ┆ year │
│ ---        ┆ ---    ┆ ---  │
│ date       ┆ f64    ┆ i32  │
╞════════════╪════════╪══════╡
│ 1981-02-23 ┆ 24.62  ┆ 1981 │
│ 1981-05-06 ┆ 27.38  ┆ 1981 │
│ 1981-05-18 ┆ 28.0   ┆ 1981 │
│ 1981-09-25 ┆ 14.25  ┆ 1981 │
│ 1982-07-08 ┆ 11.0   ┆ 1982 │
│ …          ┆ …      ┆ …    │
│ 2012-05-16 ┆ 546.08 ┆ 2012 │
│ 2012-12-04 ┆ 575.85 ┆ 2012 │
│ 2013-07-05 ┆ 417.42 ┆ 2013 │
│ 2013-11-07 ┆ 512.49 ┆ 2013 │
│ 2014-02-25 ┆ 522.06 ┆ 2014 │
└────────────┴────────┴──────┘
```
## Mixed offsets

If your data contains datetimes with mixed UTC offsets (for example due to daylight-saving
transitions), Polars parses them in UTC. You can either pass a target `time_zone` to
`str.to_datetime`, or call `str.convert_time_zone` after parsing:

  [`str.to_datetime`](https://docs.pola.rs/api/python/stable/reference/expressions/api/polars.Expr.str.to_datetime.html) ·  [`dt.convert_time_zone`](https://docs.pola.rs/api/python/stable/reference/expressions/api/polars.Expr.dt.convert_time_zone.html) ·  [Available on feature timezone](/user-guide/installation/#feature-flags)

```
data = [
    "2021-03-27T00:00:00+0100",
    "2021-03-28T00:00:00+0100",
    "2021-03-29T00:00:00+0200",
    "2021-03-30T00:00:00+0200",
]
mixed_parsed = (
    pl.Series(data)
    .str.to_datetime("%Y-%m-%dT%H:%M:%S%z")
    .dt.convert_time_zone("Europe/Brussels")
)
print(mixed_parsed)
```
  [`str.replace_all`](https://docs.pola.rs/api/rust/dev/polars_lazy/dsl/string/struct.StringNameSpace.html#method.to_datetime) ·  [`dt.convert_time_zone`](https://docs.pola.rs/api/rust/dev/polars_lazy/dsl/dt/struct.DateLikeNameSpace.html#method.convert_time_zone) ·  [Available on feature timezones](/user-guide/installation/#feature-flags) ·  [Available on feature dtype-datetime](/user-guide/installation/#feature-flags)

```
let data = [
    "2021-03-27T00:00:00+0100",
    "2021-03-28T00:00:00+0100",
    "2021-03-29T00:00:00+0200",
    "2021-03-30T00:00:00+0200",
];
let q = col("date")
    .str()
    .to_datetime(
        Some(TimeUnit::Microseconds),
        None,
        StrptimeOptions {
            format: Some("%Y-%m-%dT%H:%M:%S%z".into()),
            ..Default::default()
        },
        lit("raise"),
    )
    .dt()
    .convert_time_zone(
        TimeZone::opt_try_new(Some("Europe/Brussels"))
            .unwrap()
            .unwrap(),
    );
let mixed_parsed = df!("date" => &data)?.lazy().select([q]).collect()?;
println!("{}", &mixed_parsed);
```
```
shape: (4,)
Series: '' [datetime[μs, Europe/Brussels]]
[
    2021-03-27 00:00:00 CET
    2021-03-28 00:00:00 CET
    2021-03-29 00:00:00 CEST
    2021-03-30 00:00:00 CEST
]
```

# Citations

1. Source page: https://docs.pola.rs/user-guide/transformations/time-series/parsing
