---
type: Web Page
title: Pivots - Polars user guide
resource: https://docs.pola.rs/user-guide/transformations/pivot
timestamp: '2026-07-20T09:17:48.329595+00:00'
---

# Pivots

Pivot a column in a `DataFrame` and perform one of the following aggregations:

- first
- last
- sum
- min
- max
- mean
- median
- len

The pivot operation consists of a group by one, or multiple columns (these will be the new y-axis), the column that will be pivoted (this will be the new x-axis) and an aggregation.

## Dataset

```
shape: (5, 3)
┌─────┬─────┬─────┐
│ foo ┆ N   ┆ bar │
│ --- ┆ --- ┆ --- │
│ str ┆ i64 ┆ str │
╞═════╪═════╪═════╡
│ A   ┆ 1   ┆ k   │
│ A   ┆ 2   ┆ l   │
│ B   ┆ 2   ┆ m   │
│ B   ┆ 4   ┆ n   │
│ C   ┆ 2   ┆ o   │
└─────┴─────┴─────┘
```
## Eager

```
out = df.pivot("bar", index="foo", values="N", aggregate_function="first")
print(out)
```
```
let out = df
    .clone()
    .lazy()
    .pivot(
        Selector::ByName {
            names: [PlSmallStr::from("foo")].into(),
            strict: true,
        },
        Arc::new(df!("" => ["A", "B", "C"])?),
        Selector::ByName {
            names: [PlSmallStr::from("bar")].into(),
            strict: true,
        },
        Selector::ByName {
            names: [PlSmallStr::from("N")].into(),
            strict: true,
        },
        Expr::Agg(AggExpr::Item {
            input: Arc::new(Expr::Element),
            allow_empty: true,
        }),
        false,
        "_".into(),
        PivotColumnNaming::Auto,
    )
    .collect()?;
println!("{}", &out);
```
```
shape: (3, 6)
┌─────┬──────┬──────┬──────┬──────┬──────┐
│ foo ┆ k    ┆ l    ┆ m    ┆ n    ┆ o    │
│ --- ┆ ---  ┆ ---  ┆ ---  ┆ ---  ┆ ---  │
│ str ┆ i64  ┆ i64  ┆ i64  ┆ i64  ┆ i64  │
╞═════╪══════╪══════╪══════╪══════╪══════╡
│ A   ┆ 1    ┆ 2    ┆ null ┆ null ┆ null │
│ B   ┆ null ┆ null ┆ 2    ┆ 4    ┆ null │
│ C   ┆ null ┆ null ┆ null ┆ null ┆ 2    │
└─────┴──────┴──────┴──────┴──────┴──────┘
```
## Lazy

A Polars `LazyFrame` always need to know the schema of a computation statically (before collecting
the query). As a pivot's output schema depends on the data, and it is therefore impossible to
determine the schema without running the query.

Polars could have abstracted this fact for you just like Spark does, but we don't want you to shoot yourself in the foot with a shotgun. The cost should be clear upfront.

You have two options if you wish to use lazy evaluation with a pivot. You may collect the
`DataFrame` before performing the pivot (sacrificing lazy evaluation benefits), or you may specify
the `on_columns` parameter upfront to declare the resulting schema statically.

```
q = (
    df.lazy()
    .collect()
    .pivot(index="foo", on="bar", values="N", aggregate_function="first")
    .lazy()
)
out = q.collect()
print(out)
```
```
let q = df.clone().lazy();
let q2 = q.pivot(
    Selector::ByName {
        names: [PlSmallStr::from("foo")].into(),
        strict: true,
    },
    Arc::new(df!("" => ["A", "B", "C"])?),
    Selector::ByName {
        names: [PlSmallStr::from("bar")].into(),
        strict: true,
    },
    Selector::ByName {
        names: [PlSmallStr::from("N")].into(),
        strict: true,
    },
    Expr::Agg(AggExpr::Item {
        input: Arc::new(Expr::Element),
        allow_empty: true,
    }),
    false,
    "_".into(),
    PivotColumnNaming::Auto,
);
let out = q2.collect()?;
println!("{}", &out);
```
```
shape: (3, 6)
┌─────┬──────┬──────┬──────┬──────┬──────┐
│ foo ┆ k    ┆ l    ┆ m    ┆ n    ┆ o    │
│ --- ┆ ---  ┆ ---  ┆ ---  ┆ ---  ┆ ---  │
│ str ┆ i64  ┆ i64  ┆ i64  ┆ i64  ┆ i64  │
╞═════╪══════╪══════╪══════╪══════╪══════╡
│ A   ┆ 1    ┆ 2    ┆ null ┆ null ┆ null │
│ B   ┆ null ┆ null ┆ 2    ┆ 4    ┆ null │
│ C   ┆ null ┆ null ┆ null ┆ null ┆ 2    │
└─────┴──────┴──────┴──────┴──────┴──────┘
```
```
q = df.lazy().pivot(
    index="foo",
    on="bar",
    on_columns=["k", "l", "m", "n", "o"],
    values="N",
    aggregate_function="first",
)
out = q.collect()
print(out)
```
```
let q = df.clone().lazy();
let q2 = q.pivot(
    Selector::ByName {
        names: [PlSmallStr::from("foo")].into(),
        strict: true,
    },
    Arc::new(df!("" => ["A", "B", "C"])?),
    Selector::ByName {
        names: [PlSmallStr::from("bar")].into(),
        strict: true,
    },
    Selector::ByName {
        names: [PlSmallStr::from("N")].into(),
        strict: true,
    },
    Expr::Agg(AggExpr::Item {
        input: Arc::new(Expr::Element),
        allow_empty: true,
    }),
    false,
    "_".into(),
    PivotColumnNaming::Auto,
);
let out = q2.collect()?;
println!("{}", &out);
```
```
shape: (3, 6)
┌─────┬──────┬──────┬──────┬──────┬──────┐
│ foo ┆ k    ┆ l    ┆ m    ┆ n    ┆ o    │
│ --- ┆ ---  ┆ ---  ┆ ---  ┆ ---  ┆ ---  │
│ str ┆ i64  ┆ i64  ┆ i64  ┆ i64  ┆ i64  │
╞═════╪══════╪══════╪══════╪══════╪══════╡
│ C   ┆ null ┆ null ┆ null ┆ null ┆ 2    │
│ A   ┆ 1    ┆ 2    ┆ null ┆ null ┆ null │
│ B   ┆ null ┆ null ┆ 2    ┆ 4    ┆ null │
└─────┴──────┴──────┴──────┴──────┴──────┘
```

# Citations

1. Source page: https://docs.pola.rs/user-guide/transformations/pivot
