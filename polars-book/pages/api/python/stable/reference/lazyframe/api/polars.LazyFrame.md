---
type: Web Page
title: polars.LazyFrame.collect — Polars  documentation
resource: https://docs.pola.rs/api/python/stable/reference/lazyframe/api/polars.LazyFrame.collect.html
timestamp: '2026-09-14T12:06:43.716713+00:00'
---

# polars.LazyFrame.collect

- LazyFrame.collect(
- *** ,
- *type_coercion: [bool](https://docs.python.org/3/library/functions.html#bool) = True* ,
- *predicate_pushdown: [bool](https://docs.python.org/3/library/functions.html#bool) = True* ,
- *projection_pushdown: [bool](https://docs.python.org/3/library/functions.html#bool) = True* ,
- *simplify_expression: [bool](https://docs.python.org/3/library/functions.html#bool) = True* ,
- *slice_pushdown: [bool](https://docs.python.org/3/library/functions.html#bool) = True* ,
- *comm_subplan_elim: [bool](https://docs.python.org/3/library/functions.html#bool) = True* ,
- *comm_subexpr_elim: [bool](https://docs.python.org/3/library/functions.html#bool) = True* ,
- *cluster_with_columns: [bool](https://docs.python.org/3/library/functions.html#bool) = True* ,
- *collapse_joins: [bool](https://docs.python.org/3/library/functions.html#bool) = True* ,
- *no_optimization: [bool](https://docs.python.org/3/library/functions.html#bool) = False* ,
- *engine: EngineType = 'auto'* ,
- *background: [bool](https://docs.python.org/3/library/functions.html#bool) = False* ,
- *optimizations: [QueryOptFlags](polars.QueryOptFlags.html#polars.QueryOptFlags) = ()* ,
- ***_kwargs: Any* ,
- Materialize this `LazyFrame` into a`DataFrame` .By default, all query optimizations are enabled. Individual optimizations may be disabled by setting the corresponding parameter to `False` .
  - Parameters:
    - **type_coercion**
    - Do type coercion optimization. Deprecated since version 1.30.0: Use the `optimizations` parameters.
    - **predicate_pushdown**
    - Do predicate pushdown optimization. Deprecated since version 1.30.0: Use the `optimizations` parameters.
    - **projection_pushdown**
    - Do projection pushdown optimization. Deprecated since version 1.30.0: Use the `optimizations` parameters.
    - **simplify_expression**
    - Run simplify expressions optimization. Deprecated since version 1.30.0: Use the `optimizations` parameters.
    - **slice_pushdown**
    - Slice pushdown optimization. Deprecated since version 1.30.0: Use the `optimizations` parameters.
    - **comm_subplan_elim**
    - Will try to cache branching subplans that occur on self-joins or unions. Deprecated since version 1.30.0: Use the `optimizations` parameters.
    - **comm_subexpr_elim**
    - Common subexpressions will be cached and reused. Deprecated since version 1.30.0: Use the `optimizations` parameters.
    - **cluster_with_columns**
    - Combine sequential independent calls to with_columns Deprecated since version 1.30.0: Use the `optimizations` parameters.
    - **collapse_joins**
    - Collapse a join and filters into a faster join Deprecated since version 1.30.0: Use the `optimizations` parameters.
    - **no_optimization**
    - Turn off (certain) optimizations. Deprecated since version 1.30.0: Use the `optimizations` parameters.
    - **engine**
    - Select the engine used to process the query (default `"auto"` ).
A`Engine` instance may also be passed. Supported engine
names are:
      - `"auto"` : use the engine set by[`Config.set_engine_affinity`](../../api/polars.Config.set_engine_affinity.html#polars.Config.set_engine_affinity) or the`POLARS_ENGINE_AFFINITY` environment variable, falling
back to`"in-memory"` if unset (this default may change in
a future release).
      - `"in-memory"` : use the in-memory engine, this is the default engine.
      - `"streaming"` : use the streaming engine, which processes
queries in batches, reducing memory pressure and often
outperforming the in-memory engine. This will soon become
the default engine of Polars.
      - `"gpu"` : use the CUDA GPU engine (requires an Nvidia GPU and`cudf-polars` ). Pass a[`GPUEngine`](polars.lazyframe.engine_config.GPUEngine.html#polars.lazyframe.engine_config.GPUEngine) object for
fine-grained control (e.g. device selection on multi-GPU systems).
 If the selected engine cannot run the query, Polars falls back to the in-memory engine. Note GPU mode is considered **unstable** . Not all queries will run
successfully on the GPU, however, they should fall back transparently
to the default engine if execution is not supported.Running with `POLARS_VERBOSE=1` will provide information if a query
falls back (and why).
    - **background**
    - Run the query in the background and get a handle to the query. This handle can be used to fetch the result or cancel the query. Warning Background mode is considered **unstable** . It may be changed
at any point without it being considered a breaking change.
    - **optimizations**
    - The optimization passes done during query optimization. Warning This functionality is considered **unstable** . It may be changed
at any point without it being considered a breaking change.
  - Returns:
    - DataFrame
 See also 
  - [`explain`](polars.LazyFrame.explain.html#polars.LazyFrame.explain)
  - Print the query plan that is evaluated with collect.
  - [`profile`](polars.LazyFrame.profile.html#polars.LazyFrame.profile)
  - Collect the LazyFrame and time each node in the computation graph.
  - [`polars.collect_all`](../../api/polars.collect_all.html#polars.collect_all)
  - Collect multiple LazyFrames at the same time.
  - [`polars.Config.set_streaming_chunk_size`](../../api/polars.Config.set_streaming_chunk_size.html#polars.Config.set_streaming_chunk_size)
  - Set the size of streaming batches.
 Examples >>> lf = pl.LazyFrame( ... { ... "a": ["a", "b", "a", "b", "b", "c"], ... "b": [1, 2, 3, 4, 5, 6], ... "c": [6, 5, 4, 3, 2, 1], ... } ... ) >>> lf.group_by("a").agg(pl.all().sum()).collect() shape: (3, 3) ┌─────┬─────┬─────┐ │ a ┆ b ┆ c │ │ --- ┆ --- ┆ --- │ │ str ┆ i64 ┆ i64 │ ╞═════╪═════╪═════╡ │ a ┆ 4 ┆ 10 │ │ b ┆ 11 ┆ 10 │ │ c ┆ 6 ┆ 1 │ └─────┴─────┴─────┘ Collect in streaming mode >>> lf.group_by("a").agg(pl.all().sum()).collect( ... engine="streaming" ... ) shape: (3, 3) ┌─────┬─────┬─────┐ │ a ┆ b ┆ c │ │ --- ┆ --- ┆ --- │ │ str ┆ i64 ┆ i64 │ ╞═════╪═════╪═════╡ │ a ┆ 4 ┆ 10 │ │ b ┆ 11 ┆ 10 │ │ c ┆ 6 ┆ 1 │ └─────┴─────┴─────┘ Collect in GPU mode >>> lf.group_by("a").agg(pl.all().sum()).collect(engine="gpu") shape: (3, 3) ┌─────┬─────┬─────┐ │ a ┆ b ┆ c │ │ --- ┆ --- ┆ --- │ │ str ┆ i64 ┆ i64 │ ╞═════╪═════╪═════╡ │ b ┆ 11 ┆ 10 │ │ a ┆ 4 ┆ 10 │ │ c ┆ 6 ┆ 1 │ └─────┴─────┴─────┘ With control over the device used >>> lf.group_by("a").agg(pl.all().sum()).collect( ... engine=pl.GPUEngine(device=1) ... ) shape: (3, 3) ┌─────┬─────┬─────┐ │ a ┆ b ┆ c │ │ --- ┆ --- ┆ --- │ │ str ┆ i64 ┆ i64 │ ╞═════╪═════╪═════╡ │ b ┆ 11 ┆ 10 │ │ a ┆ 4 ┆ 10 │ │ c ┆ 6 ┆ 1 │ └─────┴─────┴─────┘

[\[source\]](https://github.com/pola-rs/polars/blob/py-1.44.2/py-polars/src/../src/polars/lazyframe/frame.py#L2382-L2596)

# Citations

1. Source page: https://docs.pola.rs/api/python/stable/reference/lazyframe/api/polars.LazyFrame.collect.html
