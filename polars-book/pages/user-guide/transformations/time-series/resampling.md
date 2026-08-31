---
type: Web Page
title: Resampling - Polars user guide
resource: https://docs.pola.rs/user-guide/transformations/time-series/resampling
timestamp: '2026-08-31T12:59:29.541052+00:00'
---

# Resampling

We can resample by either:

- upsampling (moving data to a higher frequency)
- downsampling (moving data to a lower frequency)
- combinations of these e.g. first upsample and then downsample

## Downsampling to a lower frequency

Polars views downsampling as a special case of the **group_by** operation and you can do this with
`group_by_dynamic` and `group_by_rolling` -
[see the temporal group by page for examples](../rolling/).

## Upsampling to a higher frequency

Let's go through an example where we generate data at 30 minute intervals:

```
df = pl.DataFrame(
    {
        "time": pl.datetime_range(
            start=datetime(2021, 12, 16),
            end=datetime(2021, 12, 16, 3),
            interval="30m",
            eager=True,
        ),
        "groups": ["a", "a", "a", "b", "b", "a", "a"],
        "values": [1.0, 2.0, 3.0, 4.0, 5.0, 6.0, 7.0],
    }
)
print(df)
```
  [`DataFrame`](https://docs.pola.rs/api/rust/dev/polars/frame/struct.DataFrame.html) ·  [`datetime_range`](https://docs.rs/polars/latest/polars/prelude/fn.datetime_range.html) ·  [Available on feature dtype-datetime](/user-guide/installation/#feature-flags) ·  [Available on feature lazy](/user-guide/installation/#feature-flags)

```
let time = polars::time::date_range(
    "time".into(),
    NaiveDate::from_ymd_opt(2021, 12, 16)
        .unwrap()
        .and_hms_opt(0, 0, 0)
        .unwrap(),
    NaiveDate::from_ymd_opt(2021, 12, 16)
        .unwrap()
        .and_hms_opt(3, 0, 0)
        .unwrap(),
    Duration::parse("30m"),
    ClosedWindow::Both,
    TimeUnit::Milliseconds,
    None,
)?;
let df = df!(
    "time" => time,
    "groups" => &["a", "a", "a", "b", "b", "a", "a"],
    "values" => &[1.0, 2.0, 3.0, 4.0, 5.0, 6.0, 7.0],
)?;
println!("{}", df);
```
```
shape: (7, 3)
┌─────────────────────┬────────┬────────┐
│ time                ┆ groups ┆ values │
│ ---                 ┆ ---    ┆ ---    │
│ datetime[μs]        ┆ str    ┆ f64    │
╞═════════════════════╪════════╪════════╡
│ 2021-12-16 00:00:00 ┆ a      ┆ 1.0    │
│ 2021-12-16 00:30:00 ┆ a      ┆ 2.0    │
│ 2021-12-16 01:00:00 ┆ a      ┆ 3.0    │
│ 2021-12-16 01:30:00 ┆ b      ┆ 4.0    │
│ 2021-12-16 02:00:00 ┆ b      ┆ 5.0    │
│ 2021-12-16 02:30:00 ┆ a      ┆ 6.0    │
│ 2021-12-16 03:00:00 ┆ a      ┆ 7.0    │
└─────────────────────┴────────┴────────┘
```
Upsampling can be done by defining the new sampling interval. By upsampling we are adding in extra rows where we do not have data. As such upsampling by itself gives a DataFrame with nulls. These nulls can then be filled with a fill strategy or interpolation.

### Upsampling strategies

In this example we upsample from the original 30 minutes to 15 minutes and then use a `forward`
strategy to replace the nulls with the previous non-null value:

```
shape: (13, 3)
┌─────────────────────┬────────┬────────┐
│ time                ┆ groups ┆ values │
│ ---                 ┆ ---    ┆ ---    │
│ datetime[μs]        ┆ str    ┆ f64    │
╞═════════════════════╪════════╪════════╡
│ 2021-12-16 00:00:00 ┆ a      ┆ 1.0    │
│ 2021-12-16 00:15:00 ┆ a      ┆ 1.0    │
│ 2021-12-16 00:30:00 ┆ a      ┆ 2.0    │
│ 2021-12-16 00:45:00 ┆ a      ┆ 2.0    │
│ 2021-12-16 01:00:00 ┆ a      ┆ 3.0    │
│ …                   ┆ …      ┆ …      │
│ 2021-12-16 02:00:00 ┆ b      ┆ 5.0    │
│ 2021-12-16 02:15:00 ┆ b      ┆ 5.0    │
│ 2021-12-16 02:30:00 ┆ a      ┆ 6.0    │
│ 2021-12-16 02:45:00 ┆ a      ┆ 6.0    │
│ 2021-12-16 03:00:00 ┆ a      ┆ 7.0    │
└─────────────────────┴────────┴────────┘
```
In this example we instead fill the nulls by linear interpolation:

  [`upsample`](https://docs.pola.rs/api/python/stable/reference/dataframe/api/polars.DataFrame.upsample.html) ·  [`interpolate`](https://docs.pola.rs/api/python/stable/reference/dataframe/api/polars.DataFrame.interpolate.html) ·  [`fill_null`](https://docs.pola.rs/api/python/stable/reference/expressions/api/polars.Expr.fill_null.html)

```
out2 = (
    df.upsample(time_column="time", every="15m")
    .interpolate()
    .fill_null(strategy="forward")
)
print(out2)
```
  [`upsample`](https://docs.pola.rs/api/rust/dev/polars/frame/struct.DataFrame.html#method.upsample) ·  [`interpolate`](https://docs.pola.rs/api/rust/dev/polars_lazy/dsl/enum.Expr.html#method.interpolate) ·  [`fill_null`](https://docs.pola.rs/api/rust/dev/polars_lazy/dsl/enum.Expr.html#method.fill_null)

```
let out2 = df
    .upsample::<[String; 0]>([], "time", Duration::parse("15m"))?
    .lazy()
    .with_columns([col("values").interpolate(InterpolationMethod::Linear)])
    .collect()?
    .fill_null(FillNullStrategy::Forward(None))?;
println!("{}", out2);
```
```
shape: (13, 3)
┌─────────────────────┬────────┬────────┐
│ time                ┆ groups ┆ values │
│ ---                 ┆ ---    ┆ ---    │
│ datetime[μs]        ┆ str    ┆ f64    │
╞═════════════════════╪════════╪════════╡
│ 2021-12-16 00:00:00 ┆ a      ┆ 1.0    │
│ 2021-12-16 00:15:00 ┆ a      ┆ 1.5    │
│ 2021-12-16 00:30:00 ┆ a      ┆ 2.0    │
│ 2021-12-16 00:45:00 ┆ a      ┆ 2.5    │
│ 2021-12-16 01:00:00 ┆ a      ┆ 3.0    │
│ …                   ┆ …      ┆ …      │
│ 2021-12-16 02:00:00 ┆ b      ┆ 5.0    │
│ 2021-12-16 02:15:00 ┆ b      ┆ 5.5    │
│ 2021-12-16 02:30:00 ┆ a      ┆ 6.0    │
│ 2021-12-16 02:45:00 ┆ a      ┆ 6.5    │
│ 2021-12-16 03:00:00 ┆ a      ┆ 7.0    │
└─────────────────────┴────────┴────────┘
```

# Citations

1. Source page: https://docs.pola.rs/user-guide/transformations/time-series/resampling
