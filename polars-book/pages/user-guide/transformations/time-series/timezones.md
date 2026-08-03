---
type: Web Page
title: Time zones - Polars user guide
resource: https://docs.pola.rs/user-guide/transformations/time-series/timezones
timestamp: '2026-08-03T09:49:29.273788+00:00'
---

# Time zones

Tom Scott

You really should never, ever deal with time zones if you can help it.

The `Datetime` datatype can have a time zone associated with it. Examples of valid time zones are:

- `None` : no time zone, also known as "time zone naive".
- `UTC` : Coordinated Universal Time.
- `Asia/Kathmandu` : time zone in "area/location" format. See the[list of tz database time zones](https://en.wikipedia.org/wiki/List_of_tz_database_time_zones) to
  see what's available.

Caution: Fixed offsets such as +02:00, should not be used for handling time zones. It's advised to use the "Area/Location" format mentioned above, as it can manage timezones more effectively.

Note that, because a `Datetime` can only have a single time zone, it is impossible to have a column
with multiple time zones. If you are parsing data with multiple offsets, you may want to pass
`utc=True` to convert them all to a common time zone (`UTC`), see
[parsing dates and times](../parsing/).

The main methods for setting and converting between time zones are:

- `dt.convert_time_zone` : convert from one time zone to another.
- `dt.replace_time_zone` : set/unset/change time zone.

Let's look at some examples of common operations:

  [`str.to_datetime`](https://docs.pola.rs/api/python/stable/reference/expressions/api/polars.Expr.str.to_datetime.html) ·  [`dt.replace_time_zone`](https://docs.pola.rs/api/python/stable/reference/expressions/api/polars.Expr.dt.replace_time_zone.html) ·  [Available on feature timezone](/user-guide/installation/#feature-flags)

```
ts = ["2021-03-27 03:00", "2021-03-28 03:00"]
tz_naive = pl.Series("tz_naive", ts).str.to_datetime()
tz_aware = tz_naive.dt.replace_time_zone("UTC").rename("tz_aware")
time_zones_df = pl.DataFrame([tz_naive, tz_aware])
print(time_zones_df)
```
  [`str.replace_all`](https://docs.pola.rs/api/rust/dev/polars_lazy/dsl/string/struct.StringNameSpace.html#method.to_datetime) ·  [`dt.replace_time_zone`](https://docs.pola.rs/api/rust/dev/polars_lazy/dsl/dt/struct.DateLikeNameSpace.html#method.replace_time_zone) ·  [Available on feature timezones](/user-guide/installation/#feature-flags) ·  [Available on feature dtype-datetime](/user-guide/installation/#feature-flags)

```
let ts = ["2021-03-27 03:00", "2021-03-28 03:00"];
let tz_naive = Column::new("tz_naive".into(), &ts);
let time_zones_df = DataFrame::new_infer_height(vec![tz_naive])?
    .lazy()
    .select([col("tz_naive").str().to_datetime(
        Some(TimeUnit::Milliseconds),
        None,
        StrptimeOptions::default(),
        lit("raise"),
    )])
    .with_columns([col("tz_naive")
        .dt()
        .replace_time_zone(Some(TimeZone::UTC), lit("raise"), NonExistent::Raise)
        .alias("tz_aware")])
    .collect()?;
println!("{}", &time_zones_df);
```
```
shape: (2, 2)
┌─────────────────────┬─────────────────────────┐
│ tz_naive            ┆ tz_aware                │
│ ---                 ┆ ---                     │
│ datetime[μs]        ┆ datetime[μs, UTC]       │
╞═════════════════════╪═════════════════════════╡
│ 2021-03-27 03:00:00 ┆ 2021-03-27 03:00:00 UTC │
│ 2021-03-28 03:00:00 ┆ 2021-03-28 03:00:00 UTC │
└─────────────────────┴─────────────────────────┘
```
  [`dt.convert_time_zone`](https://docs.pola.rs/api/python/stable/reference/expressions/api/polars.Expr.dt.convert_time_zone.html) ·  [`dt.replace_time_zone`](https://docs.pola.rs/api/python/stable/reference/expressions/api/polars.Expr.dt.replace_time_zone.html) ·  [Available on feature timezone](/user-guide/installation/#feature-flags)

```
time_zones_operations = time_zones_df.select(
    [
        pl.col("tz_aware")
        .dt.replace_time_zone("Europe/Brussels")
        .alias("replace time zone"),
        pl.col("tz_aware")
        .dt.convert_time_zone("Asia/Kathmandu")
        .alias("convert time zone"),
        pl.col("tz_aware").dt.replace_time_zone(None).alias("unset time zone"),
    ]
)
print(time_zones_operations)
```
  [`dt.convert_time_zone`](https://docs.pola.rs/api/rust/dev/polars_lazy/dsl/dt/struct.DateLikeNameSpace.html#method.convert_time_zone) ·  [`dt.replace_time_zone`](https://docs.pola.rs/api/rust/dev/polars_lazy/dsl/dt/struct.DateLikeNameSpace.html#method.replace_time_zone) ·  [Available on feature timezones](/user-guide/installation/#feature-flags)

```
let time_zones_operations = time_zones_df
    .lazy()
    .select([
        col("tz_aware")
            .dt()
            .replace_time_zone(
                TimeZone::opt_try_new(Some("Europe/Brussels")).unwrap(),
                lit("raise"),
                NonExistent::Raise,
            )
            .alias("replace time zone"),
        col("tz_aware")
            .dt()
            .convert_time_zone(
                TimeZone::opt_try_new(Some("Asia/Kathmandu"))
                    .unwrap()
                    .unwrap(),
            )
            .alias("convert time zone"),
        col("tz_aware")
            .dt()
            .replace_time_zone(None, lit("raise"), NonExistent::Raise)
            .alias("unset time zone"),
    ])
    .collect()?;
println!("{}", &time_zones_operations);
```
```
shape: (2, 3)
┌───────────────────────────────┬──────────────────────────────┬─────────────────────┐
│ replace time zone             ┆ convert time zone            ┆ unset time zone     │
│ ---                           ┆ ---                          ┆ ---                 │
│ datetime[μs, Europe/Brussels] ┆ datetime[μs, Asia/Kathmandu] ┆ datetime[μs]        │
╞═══════════════════════════════╪══════════════════════════════╪═════════════════════╡
│ 2021-03-27 03:00:00 CET       ┆ 2021-03-27 08:45:00 +0545    ┆ 2021-03-27 03:00:00 │
│ 2021-03-28 03:00:00 CEST      ┆ 2021-03-28 08:45:00 +0545    ┆ 2021-03-28 03:00:00 │
└───────────────────────────────┴──────────────────────────────┴─────────────────────┘
```

# Citations

1. Source page: https://docs.pola.rs/user-guide/transformations/time-series/timezones
