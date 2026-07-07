---
type: Web Page
title: Folds - Polars user guide
resource: https://docs.pola.rs/user-guide/expressions/folds
timestamp: '2026-07-07T12:26:19.464100+00:00'
---

# Folds

Polars provides many expressions to perform computations across columns, like `sum_horizontal`,
`mean_horizontal`, and `min_horizontal`. However, these are just special cases of a general
algorithm called a fold, and Polars provides a general mechanism for you to compute custom folds for
when the specialised versions of Polars are not enough.

Folds computed with the function `fold` operate on the full columns for maximum speed. They utilize
the data layout very efficiently and often have vectorized execution.

## Basic example

As a first example, we will reimplement `sum_horizontal` with the function `fold`:

```
import operator
import polars as pl
df = pl.DataFrame(
    {
        "label": ["foo", "bar", "spam"],
        "a": [1, 2, 3],
        "b": [10, 20, 30],
    }
)
result = df.select(
    pl.fold(
        acc=pl.lit(0),
        function=operator.add,
        exprs=pl.col("a", "b"),
    ).alias("sum_fold"),
    pl.sum_horizontal(pl.col("a", "b")).alias("sum_horz"),
)
print(result)
```
```
use polars::lazy::dsl::sum_horizontal;
use polars::prelude::*;
let df = df!(
    "label" => ["foo", "bar", "spam"],
    "a" => [1, 2, 3],
    "b" => [10, 20, 30],
)?;
let result = df
    .clone()
    .lazy()
    .select([
        fold_exprs(
            lit(0),
            PlanCallback::new(|(acc, val)| &acc + &val),
            [col("a"), col("b")],
            false,
            None,
        )
        .alias("sum_fold"),
        sum_horizontal([col("a"), col("b")], true)?.alias("sum_horz"),
    ])
    .collect()?;
println!("{result:?}");
```
```
shape: (3, 2)
┌──────────┬──────────┐
│ sum_fold ┆ sum_horz │
│ ---      ┆ ---      │
│ i32      ┆ i64      │
╞══════════╪══════════╡
│ 11       ┆ 11       │
│ 22       ┆ 22       │
│ 33       ┆ 33       │
└──────────┴──────────┘
```
The function `fold` expects a function `f` as the parameter `function` and `f` should accept two
arguments. The first argument is the accumulated result, which we initialise as zero, and the second
argument takes the successive values of the expressions listed in the parameter `exprs`. In our
case, they're the two columns “a” and “b”.

The snippet below includes a third explicit expression that represents what the function `fold` is
doing above:

```
acc = pl.lit(0)
f = operator.add
result = df.select(
    f(f(acc, pl.col("a")), pl.col("b")),
    pl.fold(acc=acc, function=f, exprs=pl.col("a", "b")).alias("sum_fold"),
)
print(result)
```
```
let acc = lit(0);
let f = |acc: Expr, val: Expr| acc + val;
let result = df
    .clone()
    .lazy()
    .select([
        f(f(acc, col("a")), col("b")),
        fold_exprs(
            lit(0),
            PlanCallback::new(|(acc, val)| &acc + &val),
            [col("a"), col("b")],
            false,
            None,
        )
        .alias("sum_fold"),
    ])
    .collect()?;
println!("{result:?}");
```
```
shape: (3, 2)
┌─────────┬──────────┐
│ literal ┆ sum_fold │
│ ---     ┆ ---      │
│ i64     ┆ i32      │
╞═════════╪══════════╡
│ 11      ┆ 11       │
│ 22      ┆ 22       │
│ 33      ┆ 33       │
└─────────┴──────────┘
```
`fold` in Python

Most programming languages include a higher-order function that implements the algorithm that the function `fold` in Polars implements.
The Polars `fold` is very similar to Python's `functools.reduce`.
You can learn more about the power of `functools.reduce` in this article.

## The initial value `acc`

The initial value chosen for the accumulator `acc` is typically, but not always, the
identity element of the operation you want to
apply. For example, if we wanted to multiply across the columns, we would not get the correct result
if our accumulator was set to zero:

```
result = df.select(
    pl.fold(
        acc=pl.lit(0),
        function=operator.mul,
        exprs=pl.col("a", "b"),
    ).alias("prod"),
)
print(result)
```
```
let result = df
    .clone()
    .lazy()
    .select([fold_exprs(
        lit(0),
        PlanCallback::new(|(acc, val)| &acc * &val),
        [col("a"), col("b")],
        false,
        None,
    )
    .alias("prod")])
    .collect()?;
println!("{result:?}");
```
```
shape: (3, 1)
┌──────┐
│ prod │
│ ---  │
│ i32  │
╞══════╡
│ 0    │
│ 0    │
│ 0    │
└──────┘
```
To fix this, the accumulator `acc` should be set to `1`:

```
result = df.select(
    pl.fold(
        acc=pl.lit(1),
        function=operator.mul,
        exprs=pl.col("a", "b"),
    ).alias("prod"),
)
print(result)
```
```
let result = df
    .lazy()
    .select([fold_exprs(
        lit(1),
        PlanCallback::new(|(acc, val)| &acc * &val),
        [col("a"), col("b")],
        false,
        None,
    )
    .alias("prod")])
    .collect()?;
println!("{result:?}");
```
```
shape: (3, 1)
┌──────┐
│ prod │
│ ---  │
│ i32  │
╞══════╡
│ 10   │
│ 40   │
│ 90   │
└──────┘
```
## Conditional

In the case where you'd want to apply a condition/predicate across all columns in a dataframe, a fold can be a very concise way to express this.

```
df = pl.DataFrame(
    {
        "a": [1, 2, 3],
        "b": [0, 1, 2],
    }
)
result = df.filter(
    pl.fold(
        acc=pl.lit(True),
        function=lambda acc, x: acc & x,
        exprs=pl.all() > 1,
    )
)
print(result)
```
```
let df = df!(
    "a" => [1, 2, 3],
    "b" => [0, 1, 2],
)?;
let result = df
    .lazy()
    .filter(fold_exprs(
        lit(true),
        PlanCallback::new(|(acc, val)| &acc & &val),
        [col("*").gt(1)],
        false,
        None,
    ))
    .collect()?;
println!("{result:?}");
```
```
shape: (1, 2)
┌─────┬─────┐
│ a   ┆ b   │
│ --- ┆ --- │
│ i64 ┆ i64 │
╞═════╪═════╡
│ 3   ┆ 2   │
└─────┴─────┘
```
The snippet above filters all rows where all columns are greater than 1.

## Folds and string data

Folds could be used to concatenate string data. However, due to the materialization of intermediate columns, this operation will have squared complexity.

Therefore, we recommend using the function `concat_str` for this:

```
df = pl.DataFrame(
    {
        "a": ["a", "b", "c"],
        "b": [1, 2, 3],
    }
)
result = df.select(pl.concat_str(["a", "b"]))
print(result)
```
  `concat_str` ·  Available on feature concat_str

```
let df = df!(
    "a" => ["a", "b", "c"],
    "b" => [1, 2, 3],
)?;
let result = df
    .lazy()
    .select([concat_str([col("a"), col("b")], "", false)])
    .collect()?;
println!("{result:?}");
```
```
shape: (3, 1)
┌─────┐
│ a   │
│ --- │
│ str │
╞═════╡
│ a1  │
│ b2  │
│ c3  │
└─────┘
```

# Citations

1. Source page: https://docs.pola.rs/user-guide/expressions/folds
