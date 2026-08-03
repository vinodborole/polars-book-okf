---
type: Web Page
title: Filtering - Polars user guide
resource: https://docs.pola.rs/user-guide/transformations/time-series/filter
timestamp: '2026-08-03T09:49:29.273788+00:00'
---

# Filtering

Filtering date columns works in the same way as with other types of columns using the `.filter`
method.

Polars uses Python's native `datetime`, `date` and `timedelta` for equality comparisons between the
datatypes `pl.Datetime`, `pl.Date` and `pl.Duration`.

In the following example we use a time series of Apple stock prices.

```
import polars as pl
from datetime import datetime
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
## Filtering by single dates

We can filter by a single date using an equality comparison in a filter expression:

```
shape: (1, 2)
┌────────────┬───────┐
│ Date       ┆ Close │
│ ---        ┆ ---   │
│ date       ┆ f64   │
╞════════════╪═══════╡
│ 1995-10-16 ┆ 36.13 │
└────────────┴───────┘
```
Note we are using the lowercase `datetime` method rather than the uppercase `Datetime` data type.

## Filtering by a date range

We can filter by a range of dates using the `is_between` method in a filter expression with the
start and end dates:

```
filtered_range_df = df.filter(
    pl.col("Date").is_between(datetime(1995, 7, 1), datetime(1995, 11, 1)),
)
print(filtered_range_df)
```
  [`filter`](https://docs.pola.rs/api/rust/dev/polars_lazy/frame/struct.LazyFrame.html#method.filter) ·  [`is_between`](https://docs.pola.rs/api/rust/dev/polars/prelude/enum.Expr.html#method.is_between) ·  [Available on feature is_between](/user-guide/installation/#feature-flags)

```
let filtered_range_df = df
    .lazy()
    .filter(
        col("Date")
            .gt(lit(NaiveDate::from_ymd_opt(1995, 7, 1).unwrap()))
            .and(col("Date").lt(lit(NaiveDate::from_ymd_opt(1995, 11, 1).unwrap()))),
    )
    .collect()?;
println!("{}", &filtered_range_df);
```
```
shape: (2, 2)
┌────────────┬───────┐
│ Date       ┆ Close │
│ ---        ┆ ---   │
│ date       ┆ f64   │
╞════════════╪═══════╡
│ 1995-07-06 ┆ 47.0  │
│ 1995-10-16 ┆ 36.13 │
└────────────┴───────┘
```
## Filtering with negative dates

Say you are working with an archeologist and are dealing in negative dates. Polars can parse and
store them just fine, but the Python `datetime` library does not. So for filtering, you should use
attributes in the `.dt` namespace:

```
ts = pl.Series(["-1300-05-23", "-1400-03-02"]).str.to_date()
negative_dates_df = pl.DataFrame({"ts": ts, "values": [3, 4]})
negative_dates_filtered_df = negative_dates_df.filter(pl.col("ts").dt.year() < -1300)
print(negative_dates_filtered_df)
```
  [`str.replace_all`](https://docs.pola.rs/api/rust/dev/polars_lazy/dsl/string/struct.StringNameSpace.html#method.to_date) ·  [Available on feature dtype-date](/user-guide/installation/#feature-flags)

```
    let negative_dates_df = df!(
    "ts"=> &["-1300-05-23", "-1400-03-02"],
    "values"=> &[3, 4])?
    .lazy()
    .with_column(col("ts").str().to_date(StrptimeOptions::default()))
    .collect()?;
    let negative_dates_filtered_df = negative_dates_df
        .lazy()
        .filter(col("ts").dt().year().lt(-1300))
        .collect()?;
    println!("{}", &negative_dates_filtered_df);
```
```
shape: (1, 2)
┌─────────────┬────────┐
│ ts          ┆ values │
│ ---         ┆ ---    │
│ date        ┆ i64    │
╞═════════════╪════════╡
│ -1400-03-02 ┆ 4      │
└─────────────┴────────┘
```

# Citations

1. Source page: https://docs.pola.rs/user-guide/transformations/time-series/filter
