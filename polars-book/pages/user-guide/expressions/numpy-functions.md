---
type: Web Page
title: Numpy functions - Polars user guide
resource: https://docs.pola.rs/user-guide/expressions/numpy-functions
timestamp: '2026-07-09T12:17:10.704938+00:00'
---

# Numpy functions

Polars expressions support NumPy [ufuncs](https://numpy.org/doc/stable/reference/ufuncs.html). See
[the NumPy documentation for a list of all supported NumPy functions](https://numpy.org/doc/stable/reference/ufuncs.html#available-ufuncs).

This means that if a function is not provided by Polars, we can use NumPy and we still have fast columnar operations through the NumPy API.

## Example

[   DataFrame](https://docs.pola.rs/api/python/stable/reference/dataframe/index.html) ·

[·](https://numpy.org/doc/stable/reference/generated/numpy.log.html)

`log`[Available on feature numpy](/user-guide/installation/#feature-flags)

```
import polars as pl
import numpy as np
df = pl.DataFrame({"a": [1, 2, 3], "b": [4, 5, 6]})
out = df.select(np.log(pl.all()).name.suffix("_log"))
print(out)
```
```
shape: (3, 2)
┌──────────┬──────────┐
│ a_log    ┆ b_log    │
│ ---      ┆ ---      │
│ f64      ┆ f64      │
╞══════════╪══════════╡
│ 0.0      ┆ 1.386294 │
│ 0.693147 ┆ 1.609438 │
│ 1.098612 ┆ 1.791759 │
└──────────┴──────────┘
```
## Interoperability

Polars' series have support for NumPy universal functions (ufuncs) and generalized ufuncs.
Element-wise functions such as `np.exp`, `np.cos`, `np.div`, etc, all work with almost zero
overhead.

However, bear in mind that
[Polars keeps track of missing values with a separate bitmask](../missing-data/) and NumPy does not
receive this information. This can lead to a window function or a `np.convolve` giving flawed or
incomplete results, so an error will be raised if you pass a series with missing data to a
generalized ufunc. Convert a Polars series to a NumPy array with the function `to_numpy`. Missing
values will be replaced by `np.nan` during the conversion.

# Citations

1. Source page: https://docs.pola.rs/user-guide/expressions/numpy-functions
