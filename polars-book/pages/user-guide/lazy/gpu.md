---
type: Web Page
title: GPU Support - Polars user guide
resource: https://docs.pola.rs/user-guide/lazy/gpu
timestamp: '2026-07-07T12:26:19.464100+00:00'
---

# GPU Support

Polars provides an in-memory, GPU-accelerated execution engine for the Lazy API in Python using RAPIDS cuDF on NVIDIA GPUs. This functionality is available in Open Beta, is undergoing rapid development, and is currently a single GPU implementation.

If you install Polars with the GPU feature flag, you can trigger GPU-based
execution by running `.collect(engine="gpu")` instead of `.collect()`.

```
import polars as pl
df = pl.LazyFrame({"a": [1.242, 1.535]})
q = df.select(pl.col("a").round(1))
result = q.collect(engine="gpu")
print(result)
```
```
shape: (2, 1)
┌─────┐
│ a   │
│ --- │
│ f64 │
╞═════╡
│ 1.2 │
│ 1.5 │
└─────┘
```
Learn more in the GPU Support guide.

# Citations

1. Source page: https://docs.pola.rs/user-guide/lazy/gpu
