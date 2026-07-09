---
type: Web Page
title: User-defined Python functions - Polars user guide
resource: https://docs.pola.rs/user-guide/expressions/user-defined-python-functions
timestamp: '2026-07-09T12:17:10.704938+00:00'
---

# User-defined Python functions

Note

Before writing custom Python functions, consider using Polars plugins which offer better performance and integration. Check out the [expression plugins](../../plugins/expr_plugins/) documentation for custom expressions and [I/O plugins](../../plugins/io_plugins/) for custom data sources.

Polars expressions are quite powerful and flexible, so there is much less need for custom Python functions compared to other libraries. Still, you may need to pass an expression's state to a third party library or apply your black box function to data in Polars.

In this part of the documentation we'll be using two APIs that allows you to do this:

- `map_elements`- `Series`.
- `map_batches`- `Series`to the function.

## Processing individual values with `map_elements()`

Let's start with the simplest case: we want to process each value in a `Series` individually. Here
is our data:

```
df = pl.DataFrame(
    {
        "keys": ["a", "a", "b", "b"],
        "values": [10, 7, 1, 23],
    }
)
print(df)
```
```
let df = df!(
    "keys" => &["a", "a", "b", "b"],
    "values" => &[10, 7, 1, 23],
)?;
println!("{}", df);
```
```
shape: (4, 2)
┌──────┬────────┐
│ keys ┆ values │
│ ---  ┆ ---    │
│ str  ┆ i64    │
╞══════╪════════╡
│ a    ┆ 10     │
│ a    ┆ 7      │
│ b    ┆ 1      │
│ b    ┆ 23     │
└──────┴────────┘
```
We'll call `math.log()` on each individual value:

```
def my_log(value):
    return math.log(value)
out = df.select(pl.col("values").map_elements(my_log, return_dtype=pl.Float64))
print(out)
```
```
```
```
shape: (4, 1)
┌──────────┐
│ values   │
│ ---      │
│ f64      │
╞══════════╡
│ 2.302585 │
│ 1.94591  │
│ 0.0      │
│ 3.135494 │
└──────────┘
```
While this works, `map_elements()` has two problems:

- **Limited to individual items:**Often you'll want to have a calculation that needs to operate on the whole- `Series`, rather than individual items one by one.
- **Performance overhead:**Even if you do want to process each item individually, calling a function for each individual item is slow; all those extra function calls add a lot of overhead.

Let's start by solving the first problem, and then we'll see how to solve the second problem.

## Processing a whole `Series` with `map_batches()`

We want to run a custom function on the contents of a whole `Series`. For demonstration purposes,
let's say we want to calculate the difference between the mean of a `Series` and each value.

We can use the `map_batches()` API to run this function on either the full `Series` or individual
groups in a `group_by()`:

```
def diff_from_mean(series):
    # This will be very slow for non-trivial Series, since it's all Python
    # code:
    total = 0
    for value in series:
        total += value
    mean = total / len(series)
    return pl.Series([value - mean for value in series])
# Apply our custom function to a full Series with map_batches():
out = df.select(pl.col("values").map_batches(diff_from_mean, return_dtype=pl.Float64))
print("== select() with UDF ==")
print(out)
# Apply our custom function per group:
print("== group_by() with UDF ==")
out = df.group_by("keys").agg(
    pl.col("values").map_batches(diff_from_mean, return_dtype=pl.Float64)
)
print(out)
```
```
```
```
== select() with UDF ==
shape: (4, 1)
┌────────┐
│ values │
│ ---    │
│ f64    │
╞════════╡
│ -0.25  │
│ -3.25  │
│ -9.25  │
│ 12.75  │
└────────┘
== group_by() with UDF ==
shape: (2, 2)
┌──────┬───────────────┐
│ keys ┆ values        │
│ ---  ┆ ---           │
│ str  ┆ list[f64]     │
╞══════╪═══════════════╡
│ b    ┆ [-11.0, 11.0] │
│ a    ┆ [1.5, -1.5]   │
└──────┴───────────────┘
```
## Fast operations with user-defined functions

The problem with a pure-Python implementation is that it's slow. In general, you want to minimize how much Python code you call if you want fast results.

To maximize speed, you'll want to make sure that you're using a function written in a compiled
language. For numeric calculations Polars supports a pair of interfaces defined by NumPy called
["ufuncs"](https://numpy.org/doc/stable/reference/ufuncs.html) and
["generalized ufuncs"](https://numpy.org/neps/nep-0005-generalized-ufuncs.html). The former runs on
each item individually, and the latter accepts a whole NumPy array, which allows for more flexible
operations.

[NumPy](https://numpy.org/doc/stable/reference/ufuncs.html) and other libraries like
[SciPy](https://docs.scipy.org/doc/scipy/reference/special.html#module-scipy.special) come with
pre-written ufuncs you can use with Polars. For example:

```
out = df.select(pl.col("values").map_batches(np.log, return_dtype=pl.Float64))
print(out)
```
```
```
```
shape: (4, 1)
┌──────────┐
│ values   │
│ ---      │
│ f64      │
╞══════════╡
│ 2.302585 │
│ 1.94591  │
│ 0.0      │
│ 3.135494 │
└──────────┘
```
Notice that we can use `map_batches()`, because `numpy.log()` is able to run on both individual
items and on whole NumPy arrays. This means it will run much faster than our original example, since
we only have a single Python call and then all processing happens in a fast low-level language.

## Example: A fast custom function using Numba

The pre-written functions NumPy provides are helpful, but our goal is to write our own functions.
For example, let's say we want a fast version of our `diff_from_mean()` example above. The easiest
way to write this in Python is to use [Numba](https://numba.readthedocs.io/en/stable/), which allows
you to write custom functions in (a subset) of Python while still getting the benefit of compiled
code.

In particular, Numba provides a decorator called
[ @guvectorize](https://numba.readthedocs.io/en/stable/user/vectorize.html#the-guvectorize-decorator).
This creates a generalized ufunc by compiling a Python function to fast machine code, in a way that
allows it to be used by Polars.

In the following example the `diff_from_mean_numba()` will be compiled to fast machine code at
import time, which will take a little time. After that all calls to the function will run quickly.
The `Series` will be converted to a NumPy array before being passed to the function:

```
# This will be compiled to machine code, so it will be fast. The Series is
# converted to a NumPy array before being passed to the function. See the
# Numba documentation for more details:
# https://numba.readthedocs.io/en/stable/user/vectorize.html
@guvectorize([(int64[:], float64[:])], "(n)->(n)")
def diff_from_mean_numba(arr, result):
    total = 0
    for value in arr:
        total += value
    mean = total / len(arr)
    for i, value in enumerate(arr):
        result[i] = value - mean
out = df.select(
    pl.col("values").map_batches(diff_from_mean_numba, return_dtype=pl.Float64)
)
print("== select() with UDF ==")
print(out)
out = df.group_by("keys").agg(
    pl.col("values").map_batches(diff_from_mean_numba, return_dtype=pl.Float64)
)
print("== group_by() with UDF ==")
print(out)
```
```
```
```
== select() with UDF ==
shape: (4, 1)
┌────────┐
│ values │
│ ---    │
│ f64    │
╞════════╡
│ -0.25  │
│ -3.25  │
│ -9.25  │
│ 12.75  │
└────────┘
== group_by() with UDF ==
shape: (2, 2)
┌──────┬───────────────┐
│ keys ┆ values        │
│ ---  ┆ ---           │
│ str  ┆ list[f64]     │
╞══════╪═══════════════╡
│ a    ┆ [1.5, -1.5]   │
│ b    ┆ [-11.0, 11.0] │
└──────┴───────────────┘
```
## Missing data is not allowed when calling generalized ufuncs

Before being passed to a user-defined function like `diff_from_mean_numba()`, a `Series` will be
converted to a NumPy array. Unfortunately, NumPy arrays don't have a concept of missing data. If
there is missing data in the original `Series`, this means the resulting array won't actually match
the `Series`.

If you're calculating results item by item, this doesn't matter. For example, `numpy.log()` gets
called on each individual value separately, so those missing values don't change the calculation.
But if the result of a user-defined function depend on multiple values in the `Series`, it's not
clear what exactly should happen with the missing values.

Therefore, when calling generalized ufuncs such as Numba functions decorated with `@guvectorize`,
Polars will raise an error if you try to pass in a `Series` with missing data. How do you get rid of
missing data? Either [fill it in](../missing-data/) or
[drop it](https://docs.pola.rs/py-polars/html/reference/dataframe/api/polars.DataFrame.drop_nulls.html)
before calling your custom function.

## Combining multiple column values

If you want to pass multiple columns to a user-defined function, you can use `Struct`s, which are
[covered in detail in a different section](../structs/). The basic idea is to combine multiple
columns into a `Struct`, and then the function can extract the columns back out:

```
# Add two arrays together:
@guvectorize([(int64[:], int64[:], float64[:])], "(n),(n)->(n)")
def add(arr, arr2, result):
    for i in range(len(arr)):
        result[i] = arr[i] + arr2[i]
df3 = pl.DataFrame({"values_1": [1, 2, 3], "values_2": [10, 20, 30]})
out = df3.select(
    # Create a struct that has two columns in it:
    pl.struct(["values_1", "values_2"])
    # Pass the struct to a lambda that then passes the individual columns to
    # the add() function:
    .map_batches(
        lambda combined: add(
            combined.struct.field("values_1"), combined.struct.field("values_2")
        ),
        return_dtype=pl.Float64,
    )
    .alias("add_columns")
)
print(out)
```
```
```
```
shape: (3, 1)
┌─────────────┐
│ add_columns │
│ ---         │
│ f64         │
╞═════════════╡
│ 11.0        │
│ 22.0        │
│ 33.0        │
└─────────────┘
```
## Streaming calculations

Passing the full `Series` to the user-defined function has a cost: it may use a lot of memory, as
its contents are copied into a NumPy array. You can use the `is_elementwise=True` argument to
[  map_batches](https://docs.pola.rs/py-polars/html/reference/expressions/api/polars.Expr.map_batches.html)
to stream results into the function, which means it might not get all values at once.

Note

The `is_elementwise` argument can lead to incorrect results if set incorrectly.
If you set `is_elementwise=True`, make sure that your function actually operates
element-by-element (e.g. "calculate the logarithm of each value") - our example function `diff_from_mean()`, for instance, does not.

## Return types

Custom Python functions are often black boxes; Polars doesn't know what your function is doing or
what it will return. The return data type is therefore automatically inferred. We do that by waiting
for the first non-null value. That value will then be used to determine the type of the resulting
`Series`.

The mapping of Python types to Polars data types is as follows:

- `int`->- `Int64`
- `float`->- `Float64`
- `bool`->- `Boolean`
- `str`->- `String`
- `list[tp]`->- `List[tp]`(where the inner type is inferred with the same rules)
- `dict[str, [tp]]`->- `struct`
- `Any`->- `object`(Prevent this at all times)

Rust types map as follows:

- `i32`or- `i64`->- `Int64`
- `f32`or- `f64`->- `Float64`
- `bool`->- `Boolean`
- `String`or- `str`->- `String`
- `Vec<tp>`->- `List[tp]`(where the inner type is inferred with the same rules)

You can pass a `return_dtype` argument to
[  map_batches](https://docs.pola.rs/py-polars/html/reference/expressions/api/polars.Expr.map_batches.html)
if you want to override the inferred type.

# Citations

1. Source page: https://docs.pola.rs/user-guide/expressions/user-defined-python-functions
