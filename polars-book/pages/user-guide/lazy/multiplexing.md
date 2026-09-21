---
type: Web Page
title: Multiplexing queries - Polars user guide
resource: https://docs.pola.rs/user-guide/lazy/multiplexing
timestamp: '2026-09-21T12:15:53.544991+00:00'
---

# Multiplexing queries

In the [Sources and Sinks](../sources_sinks/) page, we already discussed multiplexing as a way to
split a query into multiple sinks. This page will go a bit deeper in this concept, as it is
important to understand when combining `LazyFrame`s with procedural programming constructs.

When dealing with eager dataframes, it is very common to keep state in a temporary variable. Let's
look at the following example. Below we create a `DataFrame` with 10 unique elements in a random
order (so that Polars doesn't hit any fast paths for sorted keys).

```
np.random.seed(0)
a = np.arange(0, 10)
np.random.shuffle(a)
df = pl.DataFrame({"n": a})
print(df)
```
```
shape: (10, 1)
┌─────┐
│ n   │
│ --- │
│ i64 │
╞═════╡
│ 2   │
│ 8   │
│ 4   │
│ 9   │
│ 1   │
│ 6   │
│ 7   │
│ 3   │
│ 0   │
│ 5   │
└─────┘
```
## Eager

If you deal with the Polars eager API, making a variable and iterating over that temporary
`DataFrame` gives the result you expect, as the result of the group-by is stored in `df1`. Even
though the output order is unstable, it doesn't matter as it is eagerly evaluated. The follow
snippet therefore doesn't raise and the assert passes.

```
# A group-by doesn't guarantee order
df1 = df.group_by("n").len()
# Take the lower half and the upper half in a list
out = [df1.slice(offset=i * 5, length=5) for i in range(2)]
# Assert df1 is equal to the sum of both halves
pl.testing.assert_frame_equal(df1, pl.concat(out))
```
## Lazy

Now if we tried this naively with `LazyFrame`s, this would fail.

```
lf1 = df.lazy().group_by("n").len()
out = [lf1.slice(offset=i * 5, length=5).collect() for i in range(2)]
pl.testing.assert_frame_equal(lf1.collect(), pl.concat(out))
```
```
AssertionError: DataFrames are different (value mismatch for column 'n')
[left]:  [9, 2, 0, 5, 3, 1, 7, 8, 6, 4]
[right]: [0, 9, 6, 8, 2, 5, 4, 3, 1, 7]
```
The reason this fails is that `lf1` doesn't contain the materialized result of
`df.lazy().group_by("n").len()`, it instead holds the query plan in that variable.

This means that every time we branch of this `LazyFrame` and call `collect` we re-evaluate the
group-by. Besides being expensive, this also leads to unexpected results if you assume that the
output is stable (which isn't the case here).

In the example above you are actually evaluating 2 query plans:

**Plan 1**

**Plan 2**

## Combine the query plans

To circumvent this, we must give Polars the opportunity to look at all the query plans in a single
optimization and execution pass. This can be done by passing the diverging `LazyFrame`'s to the
`collect_all` function.

```
lf1 = df.lazy().group_by("n").len()
out = [lf1.slice(offset=i * 5, length=5) for i in range(2)]
results = pl.collect_all([lf1] + out)
pl.testing.assert_frame_equal(results[0], pl.concat(results[1:]))
```
If we explain the combined queries with `pl.explain_all`, we can also observe that they are shared
under a single "SINK_MULTIPLE" evaluation and that the optimizer has recognized that parts of the
query come from the same subplan, indicated by the inserted "CACHE" nodes.

```
SINK_MULTIPLE
  PLAN 0:
    CACHE[id: 637f5f1e-271e-4ba5-84fa-0163b8c60298]
      AGGREGATE[maintain_order: false]
        [len()] BY [col("n")]
        FROM
        DF ["n"]; PROJECT */1 COLUMNS
  PLAN 1:
    SLICE[offset: 0, len: 5]
      CACHE[id: 637f5f1e-271e-4ba5-84fa-0163b8c60298]
  PLAN 2:
    SLICE[offset: 5, len: 5]
      CACHE[id: 637f5f1e-271e-4ba5-84fa-0163b8c60298]
END SINK_MULTIPLE
```
Combining related subplans in a single execution unit with `pl.collect_all` can thus lead to large
performance increases and allows diverging query plans, storing temporary tables, and a more
procedural programming style.

# Citations

1. Source page: https://docs.pola.rs/user-guide/lazy/multiplexing
