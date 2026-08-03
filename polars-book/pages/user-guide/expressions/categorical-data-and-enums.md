---
type: Web Page
title: Categorical data and enums - Polars user guide
resource: https://docs.pola.rs/user-guide/expressions/categorical-data-and-enums
timestamp: '2026-08-03T09:49:29.273788+00:00'
---

# Categorical data and enums

String values that can only take on a limited number of possible values are known as
[categorical data](https://en.wikipedia.org/wiki/Categorical_variable). Usually, the number of
possible values is much smaller than the number of rows. Some typical examples include your
nationality, the operating system of your computer, or the license that your favorite open source
project uses.

When working with categorical data you can use Polars' dedicated types, `Categorical` and `Enum`, to
make your queries more performant. On this page, we will show what the differences are between the
two data types `Categorical` and `Enum` and when to use one or the other. We also include some notes
on
[why the data types `Categorical` and `Enum` are more efficient than using the plain string values](#performance-considerations-on-categorical-data-types)
at the end of this user guide section.

In short, from a performance perspective, prefer `Enum` over `Categorical` whenever possible. In
case your requirements change along the way, you can always cast from one to the other.

## `Enum` vs `Categorical`

`Enum` and `Categorical` have a few key differences. The categories of an `Enum` are fixed, defined
up front, and can be ordered. When categories are not yet known up front or grow dynamically, you
can use `Categorical`, but then they cannot be ordered.

## Data type `Enum`

### Creating an `Enum`

The data type `Enum` is an ordered categorical data type. To use the data type `Enum` you must
specify the categories in advance to create it. Then, when creating a new series, a new dataframe,
or when casting a string column, you can use that `Enum` variant.

```
import polars as pl
bears_enum = pl.Enum(["Polar", "Panda", "Brown"])
bears = pl.Series(["Polar", "Panda", "Brown", "Brown", "Polar"], dtype=bears_enum)
print(bears)
```
```
shape: (5,)
Series: '' [enum]
[
    "Polar"
    "Panda"
    "Brown"
    "Brown"
    "Polar"
]
```
### Invalid values

Polars will raise an error if you try to specify a data type `Enum` whose categories do not include
all the values present:

```
from polars.exceptions import InvalidOperationError
try:
    bears_kind_of = pl.Series(
        ["Polar", "Panda", "Brown", "Polar", "Shark"],
        dtype=bears_enum,
    )
except InvalidOperationError as exc:
    print("InvalidOperationError:", exc)
```
```
InvalidOperationError: conversion from `str` to `enum` failed in column '' for 1 out of 5 values: ["Shark"]
Ensure that all values in the input column are present in the categories of the enum datatype.
```
If you can't know all of the possible values in advance and erroring on unknown values is
semantically wrong, you may need to [use the data type `Categorical`](#data-type-categorical).

### Category ordering and comparison

The data type `Enum` is ordered and the order is determined by the order in which you specify the
categories. The snippet below uses log levels to demonstrate where an ordered `Enum` is useful:

```
log_levels = pl.Enum(["debug", "info", "warning", "error"])
logs = pl.DataFrame(
    {
        "level": ["debug", "info", "debug", "error"],
        "message": [
            "process id: 525",
            "Service started correctly",
            "startup time: 67ms",
            "Cannot connect to DB!",
        ],
    },
    schema_overrides={
        "level": log_levels,
    },
)
non_debug_logs = logs.filter(
    pl.col("level") > "debug",
)
print(non_debug_logs)
```
```
shape: (2, 2)
┌───────┬───────────────────────────┐
│ level ┆ message                   │
│ ---   ┆ ---                       │
│ enum  ┆ str                       │
╞═══════╪═══════════════════════════╡
│ info  ┆ Service started correctly │
│ error ┆ Cannot connect to DB!     │
└───────┴───────────────────────────┘
```
This example shows that we can compare `Enum` values with a string, but this only works if the
string matches one of the `Enum` values. If we compared the column “level” with any string other
than `"debug"`, `"info"`, `"warning"`, or `"error"`, Polars would raise an exception:

```
logs.select(pl.col("level") > "Pretty bad")  # This is not a valid logging level
```
```
conversion from `str` to `enum` failed for value "Pretty bad"
This error occurred in the following expression:
    [(col("level")) > ("Pretty bad")]
```
Columns of the `Enum` data type can be compared with other columns of the same `Enum` type or
columns that hold strings, but only if all the strings are valid `Enum` values.

```
str_series = pl.Series(["info", "debug", "debug", "error"])
print(logs["level"] == str_series)
```
```
shape: (4,)
Series: 'level' [bool]
[
    false
    false
    true
    true
]
```
## Data type `Categorical`

The data type `Categorical` can be seen as a more flexible version of `Enum`. The categories don't
need to be defined up front, since the mapping grows dynamically as data is processed.

### Creating a `Categorical` `Series`

To use the data type `Categorical`, you can cast a column of strings or specify `Categorical` as the
data type of a series or dataframe column:

```
bears_cat = pl.Series(
    ["Polar", "Panda", "Brown", "Brown", "Polar"], dtype=pl.Categorical
)
print(bears_cat)
```
```
shape: (5,)
Series: '' [cat]
[
    "Polar"
    "Panda"
    "Brown"
    "Brown"
    "Polar"
]
```
Having Polars infer the categories for you may seem easier than listing the categories beforehand,
but this continuous inference and bookkeeping comes at a performance cost. That is why, whenever
possible, you should use `Enum`. You can learn why by
[reading the subsection about `Categorical` encodings](#data-type-categorical-and-encodings).

### Using `Categories` objects

By default, all `Categorical` columns share a global mapping, so independently created columns use
the same encoding and can be combined without re-encoding. For finer control, you can pass a
`pl.Categories` object when creating a column:

```
bear_categories = pl.Categories(name="bear_species", physical=pl.UInt8)
bears = pl.DataFrame(
    {"species": ["Polar", "Brown", "Panda", "Brown", "Polar"]},
    schema_overrides={"species": pl.Categorical(bear_categories)},
)
print(bears)
```
```
shape: (5, 1)
┌─────────┐
│ species │
│ ---     │
│ cat     │
╞═════════╡
│ Polar   │
│ Brown   │
│ Panda   │
│ Brown   │
│ Polar   │
└─────────┘
```
`pl.Categories` accepts the following arguments:

- `name` : identifies the mapping. Two`Categorical` columns use compatible encodings when they
  reference a`Categories` with the same name, namespace, and physical type.
- `namespace` : an optional scope for the name, useful when multiple independent category spaces
  share the same name.
- `physical` : the integer type used to store encodings.`pl.UInt32` (default) can encode over four
  billion categories;`pl.UInt16` up to 65,535;`pl.UInt8` up to 255. A smaller type reduces memory
  usage when the category count is low.

### Lexical comparison with strings

When comparing a `Categorical` column with a string, Polars will perform a lexical comparison. In
the context of strings this means which one comes first in an alphabetical ordering.

```
shape: (5, 2)
┌─────────────┬─────────────────────┐
│ categorical ┆ categorical < "Cat" │
│ ---         ┆ ---                 │
│ cat         ┆ bool                │
╞═════════════╪═════════════════════╡
│ Polar       ┆ false               │
│ Panda       ┆ false               │
│ Brown       ┆ true                │
│ Brown       ┆ true                │
│ Polar       ┆ false               │
└─────────────┴─────────────────────┘
```
You can also compare a column of strings with your `Categorical` column, and the comparison will
also be lexical:

```
shape: (5, 3)
┌─────────────┬────────┬───────────────────────┐
│ categorical ┆ string ┆ categorical == string │
│ ---         ┆ ---    ┆ ---                   │
│ cat         ┆ str    ┆ bool                  │
╞═════════════╪════════╪═══════════════════════╡
│ Polar       ┆ Panda  ┆ false                 │
│ Panda       ┆ Brown  ┆ false                 │
│ Brown       ┆ Brown  ┆ true                  │
│ Brown       ┆ Polar  ┆ false                 │
│ Polar       ┆ Polar  ┆ true                  │
└─────────────┴────────┴───────────────────────┘
```
Although it is possible to compare a string column with a categorical column, it is typically more
efficient to compare two categorical columns directly. Comparing two `Categorical` columns always
performs a lexical comparison.

### Combining `Categorical` columns

Because `Categorical` columns share a global mapping by default, combining them (e.g.
[concatenating two dataframes vertically](../../getting-started/#concatenating-dataframes)) requires
no re-encoding:

```
male_bears = pl.DataFrame(
    {
        "species": ["Polar", "Brown", "Panda"],
        "weight": [450, 500, 110],  # kg
    },
    schema_overrides={"species": pl.Categorical},
)
female_bears = pl.DataFrame(
    {
        "species": ["Brown", "Polar", "Panda"],
        "weight": [340, 200, 90],  # kg
    },
    schema_overrides={"species": pl.Categorical},
)
bears = pl.concat([male_bears, female_bears], how="vertical")
print(bears)
```
```
shape: (6, 2)
┌─────────┬────────┐
│ species ┆ weight │
│ ---     ┆ ---    │
│ cat     ┆ i64    │
╞═════════╪════════╡
│ Polar   ┆ 450    │
│ Brown   ┆ 500    │
│ Panda   ┆ 110    │
│ Brown   ┆ 340    │
│ Polar   ┆ 200    │
│ Panda   ┆ 90     │
└─────────┴────────┘
```
The same holds when using [explicit `pl.Categories` objects](#using-categories-objects): two
`Categorical` columns can be combined without re-encoding as long as they share the same
`Categories`. For a deeper look at how encodings work, see
[the performance section below](#data-type-categorical-and-encodings).

## Performance considerations on categorical data types

This part of the user guide explains why categorical data types are more performant than plain string literals.

### Encodings

Categorical data represents string data with a finite set of possible values (that is usually much smaller than the length of the column). Storing these values as plain strings wastes memory and costs performance as the same string will be repeated over and over again. Additionally, operations like joins require expensive string comparisons.

Categorical data types like `Enum` and `Categorical` represent string literals as inexpensive
numeric placeholders that are mapped to the original string literal.

As an example of a sensible encoding, Polars could choose to represent the finite set of categories as unsigned integers. With that in mind, the diagram below shows a regular string column and a possible representation of a Polars column with the categorical data type:

| String Column | Categorical Column | 
|---|---|
|  |  | 

| Series | 
|---|
| Polar | 
| Panda | 
| Brown | 
| Panda | 
| Brown | 
| Brown | 
| Polar | 

|  |  | 

| Physical | 
|---|
| 0 | 
| 1 | 
| 2 | 
| 1 | 
| 2 | 
| 2 | 
| 0 | 

| Categories | 
|---|
| Polar | 
| Panda | 
| Brown | 

The physical representation `0` in this case encodes (or maps) to the value `Polar`, the value `1`
encodes to `Panda`, and the value `2` to `Brown`. This encoding has the benefit of only storing the
string values once. Additionally, when we perform operations (e.g. sorting, counting) we can work
directly on the physical representation which is much faster than working with string data.

### Encodings for the data type `Enum` are fixed

When working with the data type `Enum` we specify the categories in advance. This way, Polars can
ensure different columns and even different datasets have the same encoding. Because of this, there
is no need for expensive re-encoding or cache lookups.

### Data type `Categorical` and encodings

The fact that the categories for the data type `Categorical` are inferred comes at a cost. The main
cost here is that we have no control over the order in which these encodings are built.

Consider appending the following two categorical series:

```
cat_bears = pl.Series(
    ["Polar", "Panda", "Brown", "Brown", "Polar"], dtype=pl.Categorical
)
cat2_series = pl.Series(
    ["Panda", "Brown", "Brown", "Polar", "Polar"], dtype=pl.Categorical
)
print(cat_bears.extend(cat2_series))
```
Polars encodes the string values in the order they appear. So, the series would look like this:

| cat_series | cat2_series | 
|---|---|
|  |  | 

|  |  | 

| Physical | 
|---|
| 0 | 
| 1 | 
| 2 | 
| 2 | 
| 0 | 

| Categories | 
|---|
| Polar | 
| Panda | 
| Brown | 

|  |  | 

| Physical | 
|---|
| 0 | 
| 1 | 
| 1 | 
| 2 | 
| 2 | 

| Categories | 
|---|
| Panda | 
| Brown | 
| Polar | 

Polars avoids this problem by using a shared global mapping for all `Categorical` columns by
default: the same string always gets the same physical value across columns, making combination
operations cheap. When you need multiple independent category spaces, use
[explicit `pl.Categories` objects](#using-categories-objects) scoped by name and namespace; columns
sharing the same `Categories` are always compatible with each other. This comes at the cost of
lookups and edits to the global mapping, which can cause locking. This does not occur for the `Enum`
because the mapping is immutable and can be shared freely.

# Citations

1. Source page: https://docs.pola.rs/user-guide/expressions/categorical-data-and-enums
