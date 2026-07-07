---
type: Web Page
title: DataType Expressions - Polars user guide
resource: https://docs.pola.rs/user-guide/lazy/datatype_exprs
timestamp: '2026-07-07T12:26:19.464100+00:00'
---

# DataType Expressions

In your lazy queries, you may want to reason about the datatypes of columns or expressions used in your queries. DataType expressions allow for the inspection and manipulation of datatypes that are used in your query. The datatypes are resolved during query planning and behave the same as static datatypes during runtime.

DataType expressions can be especially useful when you don't have full control over input data. This can occur when you try to compartmentalize code, write utility functions or are loading data from heterogeneous data sources. DataType expressions also allow you to express relations between the datatype of expressions or columns.

## Basic Usage

DataType expressions often start with `pl.dtype_of`. This allows inspecting the datatype of a column
or expression.

```
dtype_expr = pl.dtype_of("UserID")
# For debugging you can collect the output datatype in a specific context.
schema = pl.Schema({ 'UserID': pl.UInt64, 'Name': pl.String })
dtype_expr.collect_dtype(schema)
```
These expressions can be manipulated in various ways to transform them into the datatype that you need.

```
dtype_expr.wrap_in_list().collect_dtype(schema)
dtype_expr.to_signed_integer().collect_dtype(schema)
```
You can also inspect information about the datatype to use at runtime.

```
df = schema.to_frame()
df.select(
    userid_dtype_name = pl.dtype_of('UserID').display(),
    userid_is_signed  = pl.dtype_of('UserID').matches(cs.signed_integer()),
)
```
## Expressing relations between datatypes

Datatypes can help with utility functions by being able to express the relation between the output
datatype of two expressions. The following example allows you to express that `map_batches` has the
same output datatype as input datatype.

```
def inspect(expr: pl.Expr) -> pl.Expr:
    def print_and_return(s: pl.Series) -> pl.Series:
        print(s)
        return s
    return expr.map_batches(
        print_and_return,
        # Clarify that the expression returns the same datatype as the input
        # datatype.
        return_dtype=pl.dtype_of(expr),
    )
df = pl.DataFrame({
    'UserID': [1, 2, 3, 4, 5],
    'Name': ["Alice", "Bob", "Charlie", "Diana", "Ethan"],
})
df.select(inspect(pl.col('Name')))
```
```
shape: (5,)
Series: 'Name' [str]
[
    "Alice"
    "Bob"
    "Charlie"
    "Diana"
    "Ethan"
]
```
Similarly, you want to express that one column needs to be casted to the datatype of another column.

```
df = pl.DataFrame({
    'UserID': [1, 2, 3, 4, 5],
    'Name': ["Alice", "Bob", "Charlie", "Diana", "Ethan"],
}).with_columns(
    pl.col('UserID').cast(pl.dtype_of('Name'))
)
```

# Citations

1. Source page: https://docs.pola.rs/user-guide/lazy/datatype_exprs
