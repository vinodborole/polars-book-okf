---
type: Web Page
title: Grouping - Polars user guide
resource: https://docs.pola.rs/user-guide/transformations/time-series/rolling
timestamp: '2026-07-09T12:17:10.704938+00:00'
---

# Grouping

## Grouping by fixed windows

We can calculate temporal statistics using `group_by_dynamic` to group rows into days/months/years
etc.

### Annual average example

In following simple example we calculate the annual average closing price of Apple stock prices. We first load the data from CSV:

```
df = pl.read_csv("docs/assets/data/apple_stock.csv", try_parse_dates=True)
df = df.sort("Date")
print(df)
```
```
let df = CsvReadOptions::default()
    .map_parse_options(|parse_options| parse_options.with_try_parse_dates(true))
    .try_into_reader_with_file_path(Some("docs/assets/data/apple_stock.csv".into()))
    .unwrap()
    .finish()
    .unwrap()
    .sort(
        ["Date"],
        SortMultipleOptions::default().with_maintain_order(true),
    )?;
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
Info

The dates are sorted in ascending order - if they are not sorted in this way the `group_by_dynamic` output will not be correct!

To get the annual average closing price we tell `group_by_dynamic` that we want to:

- group by the `Date`column on an annual (`1y`) basis
- take the mean values of the `Close`column for each year:

```
annual_average_df = df.group_by_dynamic("Date", every="1y").agg(pl.col("Close").mean())
df_with_year = annual_average_df.with_columns(pl.col("Date").dt.year().alias("year"))
print(df_with_year)
```
[   group_by_dynamic](https://docs.pola.rs/api/rust/dev/polars_lazy/frame/struct.LazyFrame.html#method.group_by_dynamic) ·

[Available on feature dynamic_group_by](/user-guide/installation/#feature-flags)

```
let annual_average_df = df
    .lazy()
    .group_by_dynamic(
        col("Date"),
        [],
        DynamicGroupOptions {
            every: Duration::parse("1y"),
            period: Duration::parse("1y"),
            offset: Duration::parse("0"),
            ..Default::default()
        },
    )
    .agg([col("Close").mean()])
    .collect()?;
let df_with_year = annual_average_df
    .lazy()
    .with_columns([col("Date").dt().year().alias("year")])
    .collect()?;
println!("{}", &df_with_year);
```
The annual average closing price is then:

```
shape: (34, 3)
┌────────────┬───────────┬──────┐
│ Date       ┆ Close     ┆ year │
│ ---        ┆ ---       ┆ ---  │
│ date       ┆ f64       ┆ i32  │
╞════════════╪═══════════╪══════╡
│ 1981-01-01 ┆ 23.5625   ┆ 1981 │
│ 1982-01-01 ┆ 11.0      ┆ 1982 │
│ 1983-01-01 ┆ 30.543333 ┆ 1983 │
│ 1984-01-01 ┆ 27.583333 ┆ 1984 │
│ 1985-01-01 ┆ 18.166667 ┆ 1985 │
│ …          ┆ …         ┆ …    │
│ 2010-01-01 ┆ 278.265   ┆ 2010 │
│ 2011-01-01 ┆ 368.225   ┆ 2011 │
│ 2012-01-01 ┆ 560.965   ┆ 2012 │
│ 2013-01-01 ┆ 464.955   ┆ 2013 │
│ 2014-01-01 ┆ 522.06    ┆ 2014 │
└────────────┴───────────┴──────┘
```
### Parameters for `group_by_dynamic`

A dynamic window is defined by a:

- **every**: indicates the interval of the window
- **period**: indicates the duration of the window
- **offset**: can be used to offset the start of the windows

The value for `every` sets how often the groups start. The time period values are flexible - for
example we could take:

- the average over 2 year intervals by replacing `1y`with`2y`
- the average over 18 month periods by replacing `1y`with`1y6mo`

We can also use the `period` parameter to set how long the time period for each group is. For
example, if we set the `every` parameter to be `1y` and the `period` parameter to be `2y` then we
would get groups at one year intervals where each groups spanned two years.

If the `period` parameter is not specified then it is set equal to the `every` parameter so that if
the `every` parameter is set to be `1y` then each group spans `1y` as well.

Because * every* does not have to be equal to 

*, we can create many groups in a very flexible way. They may overlap or leave boundaries between them.*

**period**Let's see how the windows for some parameter combinations would look. Let's start out boring. 🥱

- every: 1 day -> `"1d"`
- period: 1 day -> `"1d"`

```
this creates adjacent windows of the same size
|--|
   |--|
      |--|
```
- every: 1 day -> `"1d"`
- period: 2 days -> `"2d"`

```
these windows have an overlap of 1 day
|----|
   |----|
      |----|
```
- every: 2 days -> `"2d"`
- period: 1 day -> `"1d"`

```
this would leave gaps between the windows
data points that in these gaps will not be a member of any group
|--|
       |--|
              |--|
```
`truncate`

The `truncate` parameter is a Boolean variable that determines what datetime value is associated
with each group in the output. In the example above the first data point is on 23rd February 1981.
If `truncate = True` (the default) then the date for the first year in the annual average is 1st
January 1981. However, if `truncate = False` then the date for the first year in the annual average
is the date of the first data point on 23rd February 1981. Note that `truncate` only affects what's
shown in the `Date` column and does not affect the window boundaries.

### Using expressions in `group_by_dynamic`

We aren't restricted to using simple aggregations like `mean` in a group by operation - we can use
the full range of expressions available in Polars.

In the snippet below we create a `date range` with every **day** (`"1d"`) in 2021 and turn this into
a `DataFrame`.

Then in the `group_by_dynamic` we create dynamic windows that start every **month** (`"1mo"`) and
have a window length of `1` month. The values that match these dynamic windows are then assigned to
that group and can be aggregated with the powerful expression API.

Below we show an example where we use **group_by_dynamic** to compute:

- the number of days until the end of the month
- the number of days in a month

[   group_by_dynamic](https://docs.pola.rs/api/python/stable/reference/dataframe/api/polars.DataFrame.group_by_dynamic.html) ·

[·](https://docs.pola.rs/api/python/stable/reference/dataframe/api/polars.DataFrame.explode.html)

`DataFrame.explode`

`date_range````
df = (
    pl.date_range(
        start=date(2021, 1, 1),
        end=date(2021, 12, 31),
        interval="1d",
        eager=True,
    )
    .alias("time")
    .to_frame()
)
out = df.group_by_dynamic("time", every="1mo", period="1mo", closed="left").agg(
    pl.col("time").cum_count().reverse().head(3).alias("day/eom"),
    ((pl.col("time") - pl.col("time").first()).last().dt.total_days() + 1).alias(
        "days_in_month"
    ),
)
print(out)
```
[   group_by_dynamic](https://docs.pola.rs/api/rust/dev/polars_lazy/frame/struct.LazyFrame.html#method.group_by_dynamic) ·

[·](https://docs.pola.rs/api/rust/dev/polars/frame/struct.DataFrame.html#method.explode)

`DataFrame.explode`[·](https://docs.pola.rs/api/rust/dev/polars_lazy/dsl/functions/fn.date_range.html)

`date_range`[Available on feature range](/user-guide/installation/#feature-flags)·

[Available on feature dynamic_group_by](/user-guide/installation/#feature-flags)·

[Available on feature dtype-date](/user-guide/installation/#feature-flags)

```
let time = polars::time::date_range(
    "time".into(),
    NaiveDate::from_ymd_opt(2021, 1, 1)
        .unwrap()
        .and_hms_opt(0, 0, 0)
        .unwrap(),
    NaiveDate::from_ymd_opt(2021, 12, 31)
        .unwrap()
        .and_hms_opt(0, 0, 0)
        .unwrap(),
    Duration::parse("1d"),
    ClosedWindow::Both,
    TimeUnit::Milliseconds,
    None,
)?
.cast(&DataType::Date)?;
let df = df!(
    "time" => time,
)?;
let out = df
    .lazy()
    .group_by_dynamic(
        col("time"),
        [],
        DynamicGroupOptions {
            every: Duration::parse("1mo"),
            period: Duration::parse("1mo"),
            offset: Duration::parse("0"),
            closed_window: ClosedWindow::Left,
            ..Default::default()
        },
    )
    .agg([
        col("time")
            .cum_count(true) // python example has false
            .reverse()
            .head(Some(3))
            .alias("day/eom"),
        ((col("time").last() - col("time").first()).map(
            // had to use map as .duration().days() is not available
            |s| {
                Ok(s.duration()?
                    .physical()
                    .iter()
                    .map(|d| d.map(|v| v / 1000 / 24 / 60 / 60))
                    .collect::<Int64Chunked>()
                    .into_column())
            },
            |_, f| Ok(Field::new(f.name().clone(), DataType::Int64)),
        ) + lit(1))
        .alias("days_in_month"),
    ])
    .collect()?;
println!("{}", &out);
```
```
shape: (12, 3)
┌────────────┬──────────────┬───────────────┐
│ time       ┆ day/eom      ┆ days_in_month │
│ ---        ┆ ---          ┆ ---           │
│ date       ┆ list[u32]    ┆ i64           │
╞════════════╪══════════════╪═══════════════╡
│ 2021-01-01 ┆ [31, 30, 29] ┆ 31            │
│ 2021-02-01 ┆ [28, 27, 26] ┆ 28            │
│ 2021-03-01 ┆ [31, 30, 29] ┆ 31            │
│ 2021-04-01 ┆ [30, 29, 28] ┆ 30            │
│ 2021-05-01 ┆ [31, 30, 29] ┆ 31            │
│ …          ┆ …            ┆ …             │
│ 2021-08-01 ┆ [31, 30, 29] ┆ 31            │
│ 2021-09-01 ┆ [30, 29, 28] ┆ 30            │
│ 2021-10-01 ┆ [31, 30, 29] ┆ 31            │
│ 2021-11-01 ┆ [30, 29, 28] ┆ 30            │
│ 2021-12-01 ┆ [31, 30, 29] ┆ 31            │
└────────────┴──────────────┴───────────────┘
```
## Grouping by rolling windows

The rolling operation, `rolling`, is another entrance to the `group_by`/`agg` context. But different
from the `group_by_dynamic` where the windows are fixed by a parameter `every` and `period`. In a
`rolling`, the windows are not fixed at all! They are determined by the values in the
`index_column`.

So imagine having a time column with the values `{2021-01-06, 2021-01-10}` and a `period="5d"` this
would create the following windows:

```
2021-01-01   2021-01-06
    |----------|
       2021-01-05   2021-01-10
             |----------|
```
Because the windows of a rolling group by are always determined by the values in the `DataFrame`
column, the number of groups is always equal to the original `DataFrame`.

## Combining group by operations

Rolling and dynamic group by operations can be combined with normal group by operations.

Below is an example with a dynamic group by.

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
    }
)
print(df)
```
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
    "groups"=> ["a", "a", "a", "b", "b", "a", "a"],
)?;
println!("{}", &df);
```
```
shape: (7, 2)
┌─────────────────────┬────────┐
│ time                ┆ groups │
│ ---                 ┆ ---    │
│ datetime[μs]        ┆ str    │
╞═════════════════════╪════════╡
│ 2021-12-16 00:00:00 ┆ a      │
│ 2021-12-16 00:30:00 ┆ a      │
│ 2021-12-16 01:00:00 ┆ a      │
│ 2021-12-16 01:30:00 ┆ b      │
│ 2021-12-16 02:00:00 ┆ b      │
│ 2021-12-16 02:30:00 ┆ a      │
│ 2021-12-16 03:00:00 ┆ a      │
└─────────────────────┴────────┘
```
```
out = df.group_by_dynamic(
    "time",
    every="1h",
    closed="both",
    group_by="groups",
    include_boundaries=True,
).agg(pl.len())
print(out)
```
[   group_by_dynamic](https://docs.pola.rs/api/rust/dev/polars_lazy/frame/struct.LazyFrame.html#method.group_by_dynamic) ·

[Available on feature dynamic_group_by](/user-guide/installation/#feature-flags)

```
let out = df
    .lazy()
    .group_by_dynamic(
        col("time"),
        [col("groups")],
        DynamicGroupOptions {
            every: Duration::parse("1h"),
            period: Duration::parse("1h"),
            offset: Duration::parse("0"),
            include_boundaries: true,
            closed_window: ClosedWindow::Both,
            ..Default::default()
        },
    )
    .agg([len()])
    .collect()?;
println!("{}", &out);
```
```
shape: (6, 5)
┌────────┬─────────────────────┬─────────────────────┬─────────────────────┬─────┐
│ groups ┆ _lower_boundary     ┆ _upper_boundary     ┆ time                ┆ len │
│ ---    ┆ ---                 ┆ ---                 ┆ ---                 ┆ --- │
│ str    ┆ datetime[μs]        ┆ datetime[μs]        ┆ datetime[μs]        ┆ u32 │
╞════════╪═════════════════════╪═════════════════════╪═════════════════════╪═════╡
│ a      ┆ 2021-12-16 00:00:00 ┆ 2021-12-16 01:00:00 ┆ 2021-12-16 00:00:00 ┆ 3   │
│ a      ┆ 2021-12-16 01:00:00 ┆ 2021-12-16 02:00:00 ┆ 2021-12-16 01:00:00 ┆ 1   │
│ a      ┆ 2021-12-16 02:00:00 ┆ 2021-12-16 03:00:00 ┆ 2021-12-16 02:00:00 ┆ 2   │
│ a      ┆ 2021-12-16 03:00:00 ┆ 2021-12-16 04:00:00 ┆ 2021-12-16 03:00:00 ┆ 1   │
│ b      ┆ 2021-12-16 01:00:00 ┆ 2021-12-16 02:00:00 ┆ 2021-12-16 01:00:00 ┆ 2   │
│ b      ┆ 2021-12-16 02:00:00 ┆ 2021-12-16 03:00:00 ┆ 2021-12-16 02:00:00 ┆ 1   │
└────────┴─────────────────────┴─────────────────────┴─────────────────────┴─────┘
```

# Citations

1. Source page: https://docs.pola.rs/user-guide/transformations/time-series/rolling
