---
type: Web Page
title: LazyFrame in polars_lazy::frame - Rust
description: Lazy abstraction over an eager `DataFrame`.
resource: https://docs.pola.rs/api/rust/dev/polars_lazy/frame/struct.LazyFrame.html
timestamp: '2026-08-17T06:58:25.170846+00:00'
---

```
pub struct LazyFrame {
    pub logical_plan: 
```
[DslPlan](../dsl/enum.DslPlan.html),
    /* private fields */
}
## Expand description

Lazy abstraction over an eager `DataFrame`.

It really is an abstraction over a logical plan. The methods of this struct will incrementally
modify a logical plan until output is requested (via [`collect`](struct.LazyFrame.html#method.collect)).

## Fields

`logical_plan:` [DslPlan](../dsl/enum.DslPlan.html)
## Implementations

## [Source](../../src/polars_lazy/dot.rs.html#5-28)
### impl [LazyFrame](struct.LazyFrame.html)

[Source](../../src/polars_lazy/dot.rs.html#5-28)

### impl [LazyFrame](struct.LazyFrame.html)

## [Source](../../src/polars_lazy/dot.rs.html#7-15)
#### pub fn [to_dot](#method.to_dot)(&self, optimized: [bool](https://doc.rust-lang.org/nightly/std/primitive.bool.html)) -> PolarsResult<[String](https://doc.rust-lang.org/nightly/alloc/string/struct.String.html)>

**crate feature `dot_diagram`** only.

[Source](../../src/polars_lazy/dot.rs.html#7-15)

#### pub fn [to_dot](#method.to_dot)(&self, optimized: [bool](https://doc.rust-lang.org/nightly/std/primitive.bool.html)) -> PolarsResult<[String](https://doc.rust-lang.org/nightly/alloc/string/struct.String.html)>

**crate feature**only.

`dot_diagram`
Get a dot language representation of the LogicalPlan.

## [Source](../../src/polars_lazy/dot.rs.html#19-27)
#### pub fn [to_dot_streaming_phys](#method.to_dot_streaming_phys)(&self, optimized: [bool](https://doc.rust-lang.org/nightly/std/primitive.bool.html)) -> PolarsResult<[String](https://doc.rust-lang.org/nightly/alloc/string/struct.String.html)>

**crate features `dot_diagram` and `streaming`** only.

[Source](../../src/polars_lazy/dot.rs.html#19-27)

#### pub fn [to_dot_streaming_phys](#method.to_dot_streaming_phys)(&self, optimized: [bool](https://doc.rust-lang.org/nightly/std/primitive.bool.html)) -> PolarsResult<[String](https://doc.rust-lang.org/nightly/alloc/string/struct.String.html)>

**crate features**only.

`dot_diagram` and `streaming`
Get a dot language representation of the streaming physical plan.

## [Source](../../src/polars_lazy/frame/cached_arenas.rs.html#8-120)
### impl [LazyFrame](struct.LazyFrame.html)

[Source](../../src/polars_lazy/frame/cached_arenas.rs.html#8-120)

### impl [LazyFrame](struct.LazyFrame.html)

[Source](../../src/polars_lazy/frame/cached_arenas.rs.html#9-15)

#### pub fn [set_cached_arena](#method.set_cached_arena)(&self, lp_arena: [Arena](../../polars_utils/arena/struct.Arena.html)<IR>, expr_arena: [Arena](../../polars_utils/arena/struct.Arena.html)<AExpr>)

[Source](../../src/polars_lazy/frame/cached_arenas.rs.html#17-38)

#### pub fn [schema_with_arenas](#method.schema_with_arenas)(
    &mut self,
    lp_arena: &mut [Arena](../../polars_utils/arena/struct.Arena.html)<IR>,
    expr_arena: &mut [Arena](../../polars_utils/arena/struct.Arena.html)<AExpr>,
) -> PolarsResult<[SchemaRef](../../polars_core/schema/type.SchemaRef.html)>

## [Source](../../src/polars_lazy/frame/cached_arenas.rs.html#45-112)
#### pub fn [collect_schema](#method.collect_schema)(&mut self) -> PolarsResult<[SchemaRef](../../polars_core/schema/type.SchemaRef.html)>

[Source](../../src/polars_lazy/frame/cached_arenas.rs.html#45-112)

#### pub fn [collect_schema](#method.collect_schema)(&mut self) -> PolarsResult<[SchemaRef](../../polars_core/schema/type.SchemaRef.html)>

Get a handle to the schema — a map from column names to data types — of the current
`LazyFrame` computation.

Returns an `Err` if the logical plan has already encountered an error (i.e., if
`self.collect()` would fail), `Ok` otherwise.

## [Source](../../src/polars_lazy/frame/exitable.rs.html#9-43)
### impl [LazyFrame](struct.LazyFrame.html)

[Source](../../src/polars_lazy/frame/exitable.rs.html#9-43)

### impl [LazyFrame](struct.LazyFrame.html)

[Source](../../src/polars_lazy/frame/exitable.rs.html#10-42)

#### pub fn [collect_concurrently](#method.collect_concurrently)(self) -> PolarsResult<[InProcessQuery](struct.InProcessQuery.html)>

**non-WebAssembly**only.

## [Source](../../src/polars_lazy/frame/mod.rs.html#88-1994)
### impl [LazyFrame](struct.LazyFrame.html)

[Source](../../src/polars_lazy/frame/mod.rs.html#88-1994)

### impl [LazyFrame](struct.LazyFrame.html)

[Source](../../src/polars_lazy/frame/mod.rs.html#109-115)

#### pub fn [from_logical_plan](#method.from_logical_plan)(logical_plan: [DslPlan](../dsl/enum.DslPlan.html), opt_state: [OptFlags](struct.OptFlags.html)) -> Self

## [Source](../../src/polars_lazy/frame/mod.rs.html#118-120)
#### pub fn [get_current_optimizations](#method.get_current_optimizations)(&self) -> [OptFlags](struct.OptFlags.html)

[Source](../../src/polars_lazy/frame/mod.rs.html#118-120)

#### pub fn [get_current_optimizations](#method.get_current_optimizations)(&self) -> [OptFlags](struct.OptFlags.html)

Get current optimizations.

## [Source](../../src/polars_lazy/frame/mod.rs.html#123-126)
#### pub fn [with_optimizations](#method.with_optimizations)(self, opt_state: [OptFlags](struct.OptFlags.html)) -> Self

[Source](../../src/polars_lazy/frame/mod.rs.html#123-126)

#### pub fn [with_optimizations](#method.with_optimizations)(self, opt_state: [OptFlags](struct.OptFlags.html)) -> Self

Set allowed optimizations.

## [Source](../../src/polars_lazy/frame/mod.rs.html#129-131)
#### pub fn [without_optimizations](#method.without_optimizations)(self) -> Self

[Source](../../src/polars_lazy/frame/mod.rs.html#129-131)

#### pub fn [without_optimizations](#method.without_optimizations)(self) -> Self

Turn off all optimizations.

## [Source](../../src/polars_lazy/frame/mod.rs.html#134-137)
#### pub fn [with_projection_pushdown](#method.with_projection_pushdown)(self, toggle: [bool](https://doc.rust-lang.org/nightly/std/primitive.bool.html)) -> Self

[Source](../../src/polars_lazy/frame/mod.rs.html#134-137)

#### pub fn [with_projection_pushdown](#method.with_projection_pushdown)(self, toggle: [bool](https://doc.rust-lang.org/nightly/std/primitive.bool.html)) -> Self

Toggle projection pushdown optimization.

## [Source](../../src/polars_lazy/frame/mod.rs.html#140-143)
#### pub fn [with_cluster_with_columns](#method.with_cluster_with_columns)(self, toggle: [bool](https://doc.rust-lang.org/nightly/std/primitive.bool.html)) -> Self

[Source](../../src/polars_lazy/frame/mod.rs.html#140-143)

#### pub fn [with_cluster_with_columns](#method.with_cluster_with_columns)(self, toggle: [bool](https://doc.rust-lang.org/nightly/std/primitive.bool.html)) -> Self

Toggle cluster with columns optimization.

## [Source](../../src/polars_lazy/frame/mod.rs.html#147-150)
#### pub fn [with_check_order](#method.with_check_order)(self, toggle: [bool](https://doc.rust-lang.org/nightly/std/primitive.bool.html)) -> Self

[Source](../../src/polars_lazy/frame/mod.rs.html#147-150)

#### pub fn [with_check_order](#method.with_check_order)(self, toggle: [bool](https://doc.rust-lang.org/nightly/std/primitive.bool.html)) -> Self

Check if operations are order dependent and unset maintaining_order if the order would not be observed.

## [Source](../../src/polars_lazy/frame/mod.rs.html#153-156)
#### pub fn [with_predicate_pushdown](#method.with_predicate_pushdown)(self, toggle: [bool](https://doc.rust-lang.org/nightly/std/primitive.bool.html)) -> Self

[Source](../../src/polars_lazy/frame/mod.rs.html#153-156)

#### pub fn [with_predicate_pushdown](#method.with_predicate_pushdown)(self, toggle: [bool](https://doc.rust-lang.org/nightly/std/primitive.bool.html)) -> Self

Toggle predicate pushdown optimization.

## [Source](../../src/polars_lazy/frame/mod.rs.html#159-162)
#### pub fn [with_type_coercion](#method.with_type_coercion)(self, toggle: [bool](https://doc.rust-lang.org/nightly/std/primitive.bool.html)) -> Self

[Source](../../src/polars_lazy/frame/mod.rs.html#159-162)

#### pub fn [with_type_coercion](#method.with_type_coercion)(self, toggle: [bool](https://doc.rust-lang.org/nightly/std/primitive.bool.html)) -> Self

Toggle type coercion optimization.

## [Source](../../src/polars_lazy/frame/mod.rs.html#165-168)
#### pub fn [with_type_check](#method.with_type_check)(self, toggle: [bool](https://doc.rust-lang.org/nightly/std/primitive.bool.html)) -> Self

[Source](../../src/polars_lazy/frame/mod.rs.html#165-168)

#### pub fn [with_type_check](#method.with_type_check)(self, toggle: [bool](https://doc.rust-lang.org/nightly/std/primitive.bool.html)) -> Self

Toggle type check optimization.

## [Source](../../src/polars_lazy/frame/mod.rs.html#171-174)
#### pub fn [with_simplify_expr](#method.with_simplify_expr)(self, toggle: [bool](https://doc.rust-lang.org/nightly/std/primitive.bool.html)) -> Self

[Source](../../src/polars_lazy/frame/mod.rs.html#171-174)

#### pub fn [with_simplify_expr](#method.with_simplify_expr)(self, toggle: [bool](https://doc.rust-lang.org/nightly/std/primitive.bool.html)) -> Self

Toggle expression simplification optimization on or off.

## [Source](../../src/polars_lazy/frame/mod.rs.html#178-181)
#### pub fn [with_comm_subplan_elim](#method.with_comm_subplan_elim)(self, toggle: [bool](https://doc.rust-lang.org/nightly/std/primitive.bool.html)) -> Self

**crate feature `cse`** only.

[Source](../../src/polars_lazy/frame/mod.rs.html#178-181)

#### pub fn [with_comm_subplan_elim](#method.with_comm_subplan_elim)(self, toggle: [bool](https://doc.rust-lang.org/nightly/std/primitive.bool.html)) -> Self

**crate feature**only.

`cse`
Toggle common subplan elimination optimization on or off

## [Source](../../src/polars_lazy/frame/mod.rs.html#185-188)
#### pub fn [with_comm_subexpr_elim](#method.with_comm_subexpr_elim)(self, toggle: [bool](https://doc.rust-lang.org/nightly/std/primitive.bool.html)) -> Self

**crate feature `cse`** only.

[Source](../../src/polars_lazy/frame/mod.rs.html#185-188)

#### pub fn [with_comm_subexpr_elim](#method.with_comm_subexpr_elim)(self, toggle: [bool](https://doc.rust-lang.org/nightly/std/primitive.bool.html)) -> Self

**crate feature**only.

`cse`
Toggle common subexpression elimination optimization on or off

## [Source](../../src/polars_lazy/frame/mod.rs.html#191-194)
#### pub fn [with_slice_pushdown](#method.with_slice_pushdown)(self, toggle: [bool](https://doc.rust-lang.org/nightly/std/primitive.bool.html)) -> Self

[Source](../../src/polars_lazy/frame/mod.rs.html#191-194)

#### pub fn [with_slice_pushdown](#method.with_slice_pushdown)(self, toggle: [bool](https://doc.rust-lang.org/nightly/std/primitive.bool.html)) -> Self

Toggle slice pushdown optimization.

[Source](../../src/polars_lazy/frame/mod.rs.html#197-200)

#### pub fn [with_streaming](#method.with_streaming)(self, toggle: [bool](https://doc.rust-lang.org/nightly/std/primitive.bool.html)) -> Self

**crate feature**only.

`streaming`
[Source](../../src/polars_lazy/frame/mod.rs.html#202-205)

#### pub fn [with_gpu](#method.with_gpu)(self, toggle: [bool](https://doc.rust-lang.org/nightly/std/primitive.bool.html)) -> Self

## [Source](../../src/polars_lazy/frame/mod.rs.html#208-211)
#### pub fn [with_row_estimate](#method.with_row_estimate)(self, toggle: [bool](https://doc.rust-lang.org/nightly/std/primitive.bool.html)) -> Self

[Source](../../src/polars_lazy/frame/mod.rs.html#208-211)

#### pub fn [with_row_estimate](#method.with_row_estimate)(self, toggle: [bool](https://doc.rust-lang.org/nightly/std/primitive.bool.html)) -> Self

Try to estimate the number of rows so that joins can determine which side to keep in memory.

## [Source](../../src/polars_lazy/frame/mod.rs.html#214-217)
#### pub fn [_with_eager](#method._with_eager)(self, toggle: [bool](https://doc.rust-lang.org/nightly/std/primitive.bool.html)) -> Self

[Source](../../src/polars_lazy/frame/mod.rs.html#214-217)

#### pub fn [_with_eager](#method._with_eager)(self, toggle: [bool](https://doc.rust-lang.org/nightly/std/primitive.bool.html)) -> Self

Run every node eagerly. This turns off multi-node optimizations.

## [Source](../../src/polars_lazy/frame/mod.rs.html#220-222)
#### pub fn [describe_plan](#method.describe_plan)(&self) -> PolarsResult<[String](https://doc.rust-lang.org/nightly/alloc/string/struct.String.html)>

[Source](../../src/polars_lazy/frame/mod.rs.html#220-222)

#### pub fn [describe_plan](#method.describe_plan)(&self) -> PolarsResult<[String](https://doc.rust-lang.org/nightly/alloc/string/struct.String.html)>

Return a String describing the naive (un-optimized) logical plan.

## [Source](../../src/polars_lazy/frame/mod.rs.html#225-227)
#### pub fn [describe_plan_tree](#method.describe_plan_tree)(&self) -> PolarsResult<[String](https://doc.rust-lang.org/nightly/alloc/string/struct.String.html)>

[Source](../../src/polars_lazy/frame/mod.rs.html#225-227)

#### pub fn [describe_plan_tree](#method.describe_plan_tree)(&self) -> PolarsResult<[String](https://doc.rust-lang.org/nightly/alloc/string/struct.String.html)>

Return a String describing the naive (un-optimized) logical plan in tree format.

## [Source](../../src/polars_lazy/frame/mod.rs.html#232-234)
#### pub fn [describe_optimized_plan](#method.describe_optimized_plan)(&self) -> PolarsResult<[String](https://doc.rust-lang.org/nightly/alloc/string/struct.String.html)>

[Source](../../src/polars_lazy/frame/mod.rs.html#232-234)

#### pub fn [describe_optimized_plan](#method.describe_optimized_plan)(&self) -> PolarsResult<[String](https://doc.rust-lang.org/nightly/alloc/string/struct.String.html)>

Return a String describing the optimized logical plan.

Returns `Err` if optimizing the logical plan fails.

## [Source](../../src/polars_lazy/frame/mod.rs.html#239-241)
#### pub fn [describe_optimized_plan_tree](#method.describe_optimized_plan_tree)(&self) -> PolarsResult<[String](https://doc.rust-lang.org/nightly/alloc/string/struct.String.html)>

[Source](../../src/polars_lazy/frame/mod.rs.html#239-241)

#### pub fn [describe_optimized_plan_tree](#method.describe_optimized_plan_tree)(&self) -> PolarsResult<[String](https://doc.rust-lang.org/nightly/alloc/string/struct.String.html)>

Return a String describing the optimized logical plan in tree format.

Returns `Err` if optimizing the logical plan fails.

## [Source](../../src/polars_lazy/frame/mod.rs.html#247-253)
#### pub fn [explain](#method.explain)(&self, optimized: [bool](https://doc.rust-lang.org/nightly/std/primitive.bool.html)) -> PolarsResult<[String](https://doc.rust-lang.org/nightly/alloc/string/struct.String.html)>

[Source](../../src/polars_lazy/frame/mod.rs.html#247-253)

#### pub fn [explain](#method.explain)(&self, optimized: [bool](https://doc.rust-lang.org/nightly/std/primitive.bool.html)) -> PolarsResult<[String](https://doc.rust-lang.org/nightly/alloc/string/struct.String.html)>

Return a String describing the logical plan.

If `optimized` is `true`, explains the optimized plan. If `optimized` is `false`,
explains the naive, un-optimized plan.

## [Source](../../src/polars_lazy/frame/mod.rs.html#294-301)
#### pub fn [sort](#method.sort)(
    self,
    by: impl [IntoVec](../../polars_core/utils/trait.IntoVec.html)<[PlSmallStr](../../polars_utils/pl_str/struct.PlSmallStr.html)>,
    sort_options: [SortMultipleOptions](../../polars_core/chunked_array/ops/sort/options/struct.SortMultipleOptions.html),
) -> Self

[Source](../../src/polars_lazy/frame/mod.rs.html#294-301)

#### pub fn [sort](#method.sort)(
    self,
    by: impl [IntoVec](../../polars_core/utils/trait.IntoVec.html)<[PlSmallStr](../../polars_utils/pl_str/struct.PlSmallStr.html)>,
    sort_options: [SortMultipleOptions](../../polars_core/chunked_array/ops/sort/options/struct.SortMultipleOptions.html),
) -> Self

Add a sort operation to the logical plan.

Sorts the LazyFrame by the column name specified using the provided options.

#####Example

Sort DataFrame by ‘sepal_width’ column:

```
fn sort_by_a(df: DataFrame) -> LazyFrame {
    df.lazy().sort(["sepal_width"], Default::default())
}
```
Sort by a single column with specific order:

```
fn sort_with_specific_order(df: DataFrame, descending: bool) -> LazyFrame {
    df.lazy().sort(
        ["sepal_width"],
        SortMultipleOptions::new()
            .with_order_descending(descending)
    )
}
```
Sort by multiple columns with specifying order for each column:

```
fn sort_by_multiple_columns_with_specific_order(df: DataFrame) -> LazyFrame {
    df.lazy().sort(
        ["sepal_width", "sepal_length"],
        SortMultipleOptions::new()
            .with_order_descending_multi([false, true])
    )
}
```
See [`SortMultipleOptions`](../../polars_core/chunked_array/ops/sort/options/struct.SortMultipleOptions.html) for more options.

## [Source](../../src/polars_lazy/frame/mod.rs.html#322-335)
#### pub fn [sort_by_exprs](#method.sort_by_exprs)<E: [AsRef](https://doc.rust-lang.org/nightly/core/convert/trait.AsRef.html)<[[Expr](../dsl/enum.Expr.html)]>>(
    self,
    by_exprs: E,
    sort_options: [SortMultipleOptions](../../polars_core/chunked_array/ops/sort/options/struct.SortMultipleOptions.html),
) -> Self

[Source](../../src/polars_lazy/frame/mod.rs.html#322-335)

#### pub fn [sort_by_exprs](#method.sort_by_exprs)<E: [AsRef](https://doc.rust-lang.org/nightly/core/convert/trait.AsRef.html)<[[Expr](../dsl/enum.Expr.html)]>>(
    self,
    by_exprs: E,
    sort_options: [SortMultipleOptions](../../polars_core/chunked_array/ops/sort/options/struct.SortMultipleOptions.html),
) -> Self

Add a sort operation to the logical plan.

Sorts the LazyFrame by the provided list of expressions, which will be turned into concrete columns before sorting.

See [`SortMultipleOptions`](../../polars_core/chunked_array/ops/sort/options/struct.SortMultipleOptions.html) for more options.

#####Example

```
use polars_core::prelude::*;
use polars_lazy::prelude::*;
/// Sort DataFrame by 'sepal_width' column
fn example(df: DataFrame) -> LazyFrame {
      df.lazy()
        .sort_by_exprs(vec![col("sepal_width")], Default::default())
}
```
[Source](../../src/polars_lazy/frame/mod.rs.html#337-349)

#### pub fn [top_k](#method.top_k)<E: [AsRef](https://doc.rust-lang.org/nightly/core/convert/trait.AsRef.html)<[[Expr](../dsl/enum.Expr.html)]>>(
    self,
    k: [IdxSize](../../polars_utils/index/type.IdxSize.html),
    by_exprs: E,
    sort_options: [SortMultipleOptions](../../polars_core/chunked_array/ops/sort/options/struct.SortMultipleOptions.html),
) -> Self

[Source](../../src/polars_lazy/frame/mod.rs.html#351-360)

#### pub fn [bottom_k](#method.bottom_k)<E: [AsRef](https://doc.rust-lang.org/nightly/core/convert/trait.AsRef.html)<[[Expr](../dsl/enum.Expr.html)]>>(
    self,
    k: [IdxSize](../../polars_utils/index/type.IdxSize.html),
    by_exprs: E,
    sort_options: [SortMultipleOptions](../../polars_core/chunked_array/ops/sort/options/struct.SortMultipleOptions.html),
) -> Self

## [Source](../../src/polars_lazy/frame/mod.rs.html#377-379)
#### pub fn [reverse](#method.reverse)(self) -> Self

[Source](../../src/polars_lazy/frame/mod.rs.html#377-379)

#### pub fn [reverse](#method.reverse)(self) -> Self

Reverse the `DataFrame` from top to bottom.

Row `i` becomes row `number_of_rows - i - 1`.

#####Example

```
use polars_core::prelude::*;
use polars_lazy::prelude::*;
fn example(df: DataFrame) -> LazyFrame {
      df.lazy()
        .reverse()
}
```
## [Source](../../src/polars_lazy/frame/mod.rs.html#388-416)
#### pub fn [rename](#method.rename)<I, J, T, S>(self, existing: I, new: J, strict: [bool](https://doc.rust-lang.org/nightly/std/primitive.bool.html)) -> Self

[Source](../../src/polars_lazy/frame/mod.rs.html#388-416)

#### pub fn [rename](#method.rename)<I, J, T, S>(self, existing: I, new: J, strict: [bool](https://doc.rust-lang.org/nightly/std/primitive.bool.html)) -> Self

Rename columns in the DataFrame.

`existing` and `new` are iterables of the same length containing the old and
corresponding new column names. Renaming happens to all `existing` columns
simultaneously, not iteratively. If `strict` is true, all columns in `existing`
must be present in the `LazyFrame` when `rename` is called; otherwise, only
those columns that are actually found will be renamed (others will be ignored).

## [Source](../../src/polars_lazy/frame/mod.rs.html#424-428)
#### pub fn [drop](#method.drop)(self, columns: [Selector](../dsl/enum.Selector.html)) -> Self

[Source](../../src/polars_lazy/frame/mod.rs.html#424-428)

#### pub fn [drop](#method.drop)(self, columns: [Selector](../dsl/enum.Selector.html)) -> Self

Removes columns from the DataFrame. Note that it’s better to only select the columns you need and let the projection pushdown optimize away the unneeded columns.

Any given columns that are not in the schema will give a [`PolarsError::ColumnNotFound`]
error while materializing the [`LazyFrame`](struct.LazyFrame.html).

## [Source](../../src/polars_lazy/frame/mod.rs.html#434-436)
#### pub fn [shift](#method.shift)<E: [Into](https://doc.rust-lang.org/nightly/core/convert/trait.Into.html)<[Expr](../dsl/enum.Expr.html)>>(self, n: E) -> Self

[Source](../../src/polars_lazy/frame/mod.rs.html#434-436)

#### pub fn [shift](#method.shift)<E: [Into](https://doc.rust-lang.org/nightly/core/convert/trait.Into.html)<[Expr](../dsl/enum.Expr.html)>>(self, n: E) -> Self

Shift the values by a given period and fill the parts that will be empty due to this operation
with `Nones`.

See the method on [Series](../../polars_core/series/series_trait/trait.SeriesTrait.html#tymethod.shift) for more info on the `shift` operation.

## [Source](../../src/polars_lazy/frame/mod.rs.html#442-446)
#### pub fn [shift_and_fill](#method.shift_and_fill)<E: [Into](https://doc.rust-lang.org/nightly/core/convert/trait.Into.html)<[Expr](../dsl/enum.Expr.html)>, IE: [Into](https://doc.rust-lang.org/nightly/core/convert/trait.Into.html)<[Expr](../dsl/enum.Expr.html)>>(
    self,
    n: E,
    fill_value: IE,
) -> Self

[Source](../../src/polars_lazy/frame/mod.rs.html#442-446)

#### pub fn [shift_and_fill](#method.shift_and_fill)<E: [Into](https://doc.rust-lang.org/nightly/core/convert/trait.Into.html)<[Expr](../dsl/enum.Expr.html)>, IE: [Into](https://doc.rust-lang.org/nightly/core/convert/trait.Into.html)<[Expr](../dsl/enum.Expr.html)>>(
    self,
    n: E,
    fill_value: IE,
) -> Self

Shift the values by a given period and fill the parts that will be empty due to this operation
with the result of the `fill_value` expression.

See the method on [Series](../../polars_core/series/series_trait/trait.SeriesTrait.html#tymethod.shift) for more info on the `shift` operation.

## [Source](../../src/polars_lazy/frame/mod.rs.html#449-453)
#### pub fn [fill_null](#method.fill_null)<E: [Into](https://doc.rust-lang.org/nightly/core/convert/trait.Into.html)<[Expr](../dsl/enum.Expr.html)>>(self, fill_value: E) -> [LazyFrame](struct.LazyFrame.html)

[Source](../../src/polars_lazy/frame/mod.rs.html#449-453)

#### pub fn [fill_null](#method.fill_null)<E: [Into](https://doc.rust-lang.org/nightly/core/convert/trait.Into.html)<[Expr](../dsl/enum.Expr.html)>>(self, fill_value: E) -> [LazyFrame](struct.LazyFrame.html)

Fill None values in the DataFrame with an expression.

## [Source](../../src/polars_lazy/frame/mod.rs.html#456-460)
#### pub fn [fill_nan](#method.fill_nan)<E: [Into](https://doc.rust-lang.org/nightly/core/convert/trait.Into.html)<[Expr](../dsl/enum.Expr.html)>>(self, fill_value: E) -> [LazyFrame](struct.LazyFrame.html)

[Source](../../src/polars_lazy/frame/mod.rs.html#456-460)

#### pub fn [fill_nan](#method.fill_nan)<E: [Into](https://doc.rust-lang.org/nightly/core/convert/trait.Into.html)<[Expr](../dsl/enum.Expr.html)>>(self, fill_value: E) -> [LazyFrame](struct.LazyFrame.html)

Fill NaN values in the DataFrame with an expression.

## [Source](../../src/polars_lazy/frame/mod.rs.html#465-469)
#### pub fn [cache](#method.cache)(self) -> Self

[Source](../../src/polars_lazy/frame/mod.rs.html#465-469)

#### pub fn [cache](#method.cache)(self) -> Self

Caches the result into a new LazyFrame.

This should be used to prevent computations running multiple times.

## [Source](../../src/polars_lazy/frame/mod.rs.html#472-491)
#### pub fn [cast](#method.cast)(self, dtypes: [PlHashMap](../../polars_utils/aliases/type.PlHashMap.html)<&[str](https://doc.rust-lang.org/nightly/std/primitive.str.html), [DataType](../../polars_core/datatypes/dtype/enum.DataType.html)>, strict: [bool](https://doc.rust-lang.org/nightly/std/primitive.bool.html)) -> Self

[Source](../../src/polars_lazy/frame/mod.rs.html#472-491)

#### pub fn [cast](#method.cast)(self, dtypes: [PlHashMap](../../polars_utils/aliases/type.PlHashMap.html)<&[str](https://doc.rust-lang.org/nightly/std/primitive.str.html), [DataType](../../polars_core/datatypes/dtype/enum.DataType.html)>, strict: [bool](https://doc.rust-lang.org/nightly/std/primitive.bool.html)) -> Self

Cast named frame columns, resulting in a new LazyFrame with updated dtypes

## [Source](../../src/polars_lazy/frame/mod.rs.html#494-500)
#### pub fn [cast_all](#method.cast_all)(self, dtype: impl [Into](https://doc.rust-lang.org/nightly/core/convert/trait.Into.html)<[DataTypeExpr](../dsl/enum.DataTypeExpr.html)>, strict: [bool](https://doc.rust-lang.org/nightly/std/primitive.bool.html)) -> Self

[Source](../../src/polars_lazy/frame/mod.rs.html#494-500)

#### pub fn [cast_all](#method.cast_all)(self, dtype: impl [Into](https://doc.rust-lang.org/nightly/core/convert/trait.Into.html)<[DataTypeExpr](../dsl/enum.DataTypeExpr.html)>, strict: [bool](https://doc.rust-lang.org/nightly/std/primitive.bool.html)) -> Self

Cast all frame columns to the given dtype, resulting in a new LazyFrame

[Source](../../src/polars_lazy/frame/mod.rs.html#502-508)

#### pub fn [optimize](#method.optimize)(
    self,
    lp_arena: &mut [Arena](../../polars_utils/arena/struct.Arena.html)<IR>,
    expr_arena: &mut [Arena](../../polars_utils/arena/struct.Arena.html)<AExpr>,
) -> PolarsResult<[Node](../../polars_utils/arena/struct.Node.html)>

[Source](../../src/polars_lazy/frame/mod.rs.html#510-515)

#### pub fn [to_alp_optimized](#method.to_alp_optimized)(self) -> PolarsResult<IRPlan>

[Source](../../src/polars_lazy/frame/mod.rs.html#517-527)

#### pub fn [to_alp](#method.to_alp)(self) -> PolarsResult<IRPlan>

[Source](../../src/polars_lazy/frame/mod.rs.html#609-621)

#### pub fn [_collect_post_opt](#method._collect_post_opt)<P>(self, post_opt: P) -> PolarsResult<[DataFrame](../../polars_core/frame/dataframe/struct.DataFrame.html)>

## [Source](../../src/polars_lazy/frame/mod.rs.html#636-696)
#### pub fn [collect_with_engine](#method.collect_with_engine)(self, engine: [Engine](../dsl/enum.Engine.html)) -> PolarsResult<[QueryResult](../../polars_core/query_result/enum.QueryResult.html)>

[Source](../../src/polars_lazy/frame/mod.rs.html#636-696)

#### pub fn [collect_with_engine](#method.collect_with_engine)(self, engine: [Engine](../dsl/enum.Engine.html)) -> PolarsResult<[QueryResult](../../polars_core/query_result/enum.QueryResult.html)>

Execute all the lazy operations and collect them into a [`DataFrame`](../../polars_core/frame/dataframe/struct.DataFrame.html) using a specified
`engine`.

The query is optimized prior to execution.

[Source](../../src/polars_lazy/frame/mod.rs.html#698-705)

#### pub fn [explain_all](#method.explain_all)(
    plans: [Vec](https://doc.rust-lang.org/nightly/alloc/vec/struct.Vec.html)<[DslPlan](../dsl/enum.DslPlan.html)>,
    opt_state: [OptFlags](struct.OptFlags.html),
) -> PolarsResult<[String](https://doc.rust-lang.org/nightly/alloc/string/struct.String.html)>

[Source](../../src/polars_lazy/frame/mod.rs.html#707-723)

#### pub fn [collect_all_with_engine](#method.collect_all_with_engine)(
    plans: [Vec](https://doc.rust-lang.org/nightly/alloc/vec/struct.Vec.html)<[DslPlan](../dsl/enum.DslPlan.html)>,
    engine: [Engine](../dsl/enum.Engine.html),
    opt_state: [OptFlags](struct.OptFlags.html),
) -> PolarsResult<[Vec](https://doc.rust-lang.org/nightly/alloc/vec/struct.Vec.html)<[DataFrame](../../polars_core/frame/dataframe/struct.DataFrame.html)>>

## [Source](../../src/polars_lazy/frame/mod.rs.html#742-748)
#### pub fn [collect](#method.collect)(self) -> PolarsResult<[DataFrame](../../polars_core/frame/dataframe/struct.DataFrame.html)>

[Source](../../src/polars_lazy/frame/mod.rs.html#742-748)

#### pub fn [collect](#method.collect)(self) -> PolarsResult<[DataFrame](../../polars_core/frame/dataframe/struct.DataFrame.html)>

Execute all the lazy operations and collect them into a [`DataFrame`](../../polars_core/frame/dataframe/struct.DataFrame.html).

The query is optimized prior to execution.

#####Example

```
use polars_core::prelude::*;
use polars_lazy::prelude::*;
fn example(df: DataFrame) -> PolarsResult<DataFrame> {
    df.lazy()
      .group_by([col("foo")])
      .agg([col("bar").sum(), col("ham").mean().alias("avg_ham")])
      .collect()
}
```
## [Source](../../src/polars_lazy/frame/mod.rs.html#755-790)
#### pub fn [collect_batches](#method.collect_batches)(
    self,
    engine: [Engine](../dsl/enum.Engine.html),
    maintain_order: [bool](https://doc.rust-lang.org/nightly/std/primitive.bool.html),
    chunk_size: [Option](https://doc.rust-lang.org/nightly/core/option/enum.Option.html)<[NonZeroUsize](https://doc.rust-lang.org/nightly/core/num/nonzero/type.NonZeroUsize.html)>,
    lazy: [bool](https://doc.rust-lang.org/nightly/std/primitive.bool.html),
) -> PolarsResult<[CollectBatches](struct.CollectBatches.html)>

**crate feature `async`** only.

[Source](../../src/polars_lazy/frame/mod.rs.html#755-790)

#### pub fn [collect_batches](#method.collect_batches)(
    self,
    engine: [Engine](../dsl/enum.Engine.html),
    maintain_order: [bool](https://doc.rust-lang.org/nightly/std/primitive.bool.html),
    chunk_size: [Option](https://doc.rust-lang.org/nightly/core/option/enum.Option.html)<[NonZeroUsize](https://doc.rust-lang.org/nightly/core/num/nonzero/type.NonZeroUsize.html)>,
    lazy: [bool](https://doc.rust-lang.org/nightly/std/primitive.bool.html),
) -> PolarsResult<[CollectBatches](struct.CollectBatches.html)>

**crate feature**only.

`async`
Collect the query in batches.

If lazy is true the query will not start until the first poll (or until start is called on CollectBatches).

[Source](../../src/polars_lazy/frame/mod.rs.html#794-810)

#### pub fn [_profile_post_opt](#method._profile_post_opt)<P>(
    self,
    post_opt: P,
) -> PolarsResult<([DataFrame](../../polars_core/frame/dataframe/struct.DataFrame.html), [DataFrame](../../polars_core/frame/dataframe/struct.DataFrame.html))>

## [Source](../../src/polars_lazy/frame/mod.rs.html#819-821)
#### pub fn [profile](#method.profile)(self) -> PolarsResult<([DataFrame](../../polars_core/frame/dataframe/struct.DataFrame.html), [DataFrame](../../polars_core/frame/dataframe/struct.DataFrame.html))>

[Source](../../src/polars_lazy/frame/mod.rs.html#819-821)

#### pub fn [profile](#method.profile)(self) -> PolarsResult<([DataFrame](../../polars_core/frame/dataframe/struct.DataFrame.html), [DataFrame](../../polars_core/frame/dataframe/struct.DataFrame.html))>

Profile a LazyFrame.

This will run the query and return a tuple containing the materialized DataFrame and a DataFrame that contains profiling information of each node that is executed.

The units of the timings are microseconds.

[Source](../../src/polars_lazy/frame/mod.rs.html#823-846)

#### pub fn [sink_batches](#method.sink_batches)(
    self,
    function: [PlanCallback](../prelude/enum.PlanCallback.html)<[DataFrame](../../polars_core/frame/dataframe/struct.DataFrame.html), [bool](https://doc.rust-lang.org/nightly/std/primitive.bool.html)>,
    maintain_order: [bool](https://doc.rust-lang.org/nightly/std/primitive.bool.html),
    chunk_size: [Option](https://doc.rust-lang.org/nightly/core/option/enum.Option.html)<[NonZeroUsize](https://doc.rust-lang.org/nightly/core/num/nonzero/type.NonZeroUsize.html)>,
) -> PolarsResult<Self>

[Source](../../src/polars_lazy/frame/mod.rs.html#891-928)

#### pub fn [sink](#method.sink)(
    self,
    sink_type: [SinkDestination](../dsl/enum.SinkDestination.html),
    file_format: [FileWriteFormat](../dsl/enum.FileWriteFormat.html),
    unified_sink_args: [UnifiedSinkArgs](../dsl/struct.UnifiedSinkArgs.html),
) -> PolarsResult<Self>

## [Source](../../src/polars_lazy/frame/mod.rs.html#947-951)
#### pub fn [filter](#method.filter)(self, predicate: [Expr](../dsl/enum.Expr.html)) -> Self

[Source](../../src/polars_lazy/frame/mod.rs.html#947-951)

#### pub fn [filter](#method.filter)(self, predicate: [Expr](../dsl/enum.Expr.html)) -> Self

Filter frame rows that match a predicate expression.

The expression must yield boolean values (note that rows where the
predicate resolves to `null` are *not* included in the resulting frame).

#####Example

```
use polars_core::prelude::*;
use polars_lazy::prelude::*;
fn example(df: DataFrame) -> LazyFrame {
      df.lazy()
        .filter(col("sepal_width").is_not_null())
        .select([col("sepal_width"), col("sepal_length")])
}
```
## [Source](../../src/polars_lazy/frame/mod.rs.html#970-972)
#### pub fn [remove](#method.remove)(self, predicate: [Expr](../dsl/enum.Expr.html)) -> Self

[Source](../../src/polars_lazy/frame/mod.rs.html#970-972)

#### pub fn [remove](#method.remove)(self, predicate: [Expr](../dsl/enum.Expr.html)) -> Self

Remove frame rows that match a predicate expression.

The expression must yield boolean values (note that rows where the
predicate resolves to `null` are *not* removed from the resulting frame).

#####Example

```
use polars_core::prelude::*;
use polars_lazy::prelude::*;
fn example(df: DataFrame) -> LazyFrame {
      df.lazy()
        .remove(col("sepal_width").is_null())
        .select([col("sepal_width"), col("sepal_length")])
}
```
## [Source](../../src/polars_lazy/frame/mod.rs.html#999-1009)
#### pub fn [select](#method.select)<E: [AsRef](https://doc.rust-lang.org/nightly/core/convert/trait.AsRef.html)<[[Expr](../dsl/enum.Expr.html)]>>(self, exprs: E) -> Self

[Source](../../src/polars_lazy/frame/mod.rs.html#999-1009)

#### pub fn [select](#method.select)<E: [AsRef](https://doc.rust-lang.org/nightly/core/convert/trait.AsRef.html)<[[Expr](../dsl/enum.Expr.html)]>>(self, exprs: E) -> Self

Select (and optionally rename, with [`alias`](../dsl/enum.Expr.html#method.alias)) columns from the query.

Columns can be selected with [`col`](../dsl/functions/fn.col.html);
If you want to select all columns use `col(PlSmallStr::from_static("*"))`.

#####Example

```
use polars_core::prelude::*;
use polars_lazy::prelude::*;
/// This function selects column "foo" and column "bar".
/// Column "bar" is renamed to "ham".
fn example(df: DataFrame) -> LazyFrame {
      df.lazy()
        .select([col("foo"),
                  col("bar").alias("ham")])
}
/// This function selects all columns except "foo"
fn exclude_a_column(df: DataFrame) -> LazyFrame {
      df.lazy()
        .select([all().exclude_cols(["foo"]).as_expr()])
}
```
[Source](../../src/polars_lazy/frame/mod.rs.html#1011-1021)

#### pub fn [select_seq](#method.select_seq)<E: [AsRef](https://doc.rust-lang.org/nightly/core/convert/trait.AsRef.html)<[[Expr](../dsl/enum.Expr.html)]>>(self, exprs: E) -> Self

## [Source](../../src/polars_lazy/frame/mod.rs.html#1049-1080)
#### pub fn [group_by](#method.group_by)<E: [AsRef](https://doc.rust-lang.org/nightly/core/convert/trait.AsRef.html)<[\[IE\]](https://doc.rust-lang.org/nightly/std/primitive.slice.html)>, IE: [Into](https://doc.rust-lang.org/nightly/core/convert/trait.Into.html)<[Expr](../dsl/enum.Expr.html)> + [Clone](https://doc.rust-lang.org/nightly/core/clone/trait.Clone.html)>(
    self,
    by: E,
) -> [LazyGroupBy](struct.LazyGroupBy.html)

[Source](../../src/polars_lazy/frame/mod.rs.html#1049-1080)

#### pub fn [group_by](#method.group_by)<E: [AsRef](https://doc.rust-lang.org/nightly/core/convert/trait.AsRef.html)<[\[IE\]](https://doc.rust-lang.org/nightly/std/primitive.slice.html)>, IE: [Into](https://doc.rust-lang.org/nightly/core/convert/trait.Into.html)<[Expr](../dsl/enum.Expr.html)> + [Clone](https://doc.rust-lang.org/nightly/core/clone/trait.Clone.html)>(
    self,
    by: E,
) -> [LazyGroupBy](struct.LazyGroupBy.html)

Performs a “group-by” on a `LazyFrame`, producing a [`LazyGroupBy`](struct.LazyGroupBy.html), which can subsequently be aggregated.

Takes a list of expressions to group on.

#####Example

```
use polars_core::prelude::*;
use polars_lazy::prelude::*;
fn example(df: DataFrame) -> LazyFrame {
      df.lazy()
       .group_by([col("date")])
       .agg([
           col("rain").min().alias("min_rain"),
           col("rain").sum().alias("sum_rain"),
           col("rain").quantile(lit(0.5), QuantileMethod::Nearest).alias("median_rain"),
       ])
}
```
## [Source](../../src/polars_lazy/frame/mod.rs.html#1090-1118)
#### pub fn [rolling](#method.rolling)<E: [AsRef](https://doc.rust-lang.org/nightly/core/convert/trait.AsRef.html)<[[Expr](../dsl/enum.Expr.html)]>>(
    self,
    index_column: [Expr](../dsl/enum.Expr.html),
    group_by: E,
    options: [RollingGroupOptions](../prelude/struct.RollingGroupOptions.html),
) -> [LazyGroupBy](struct.LazyGroupBy.html)

**crate feature `dynamic_group_by`** only.

[Source](../../src/polars_lazy/frame/mod.rs.html#1090-1118)

#### pub fn [rolling](#method.rolling)<E: [AsRef](https://doc.rust-lang.org/nightly/core/convert/trait.AsRef.html)<[[Expr](../dsl/enum.Expr.html)]>>(
    self,
    index_column: [Expr](../dsl/enum.Expr.html),
    group_by: E,
    options: [RollingGroupOptions](../prelude/struct.RollingGroupOptions.html),
) -> [LazyGroupBy](struct.LazyGroupBy.html)

**crate feature**only.

`dynamic_group_by`
Create rolling groups based on a time column.

Also works for index values of type UInt32, UInt64, Int32, or Int64.

Different from a [`group_by_dynamic`](struct.LazyFrame.html#method.group_by_dynamic), the windows are now determined by the
individual values and are not of constant intervals. For constant intervals use
*group_by_dynamic*

## [Source](../../src/polars_lazy/frame/mod.rs.html#1136-1164)
#### pub fn [group_by_dynamic](#method.group_by_dynamic)<E: [AsRef](https://doc.rust-lang.org/nightly/core/convert/trait.AsRef.html)<[[Expr](../dsl/enum.Expr.html)]>>(
    self,
    index_column: [Expr](../dsl/enum.Expr.html),
    group_by: E,
    options: [DynamicGroupOptions](../prelude/struct.DynamicGroupOptions.html),
) -> [LazyGroupBy](struct.LazyGroupBy.html)

**crate feature `dynamic_group_by`** only.

[Source](../../src/polars_lazy/frame/mod.rs.html#1136-1164)

#### pub fn [group_by_dynamic](#method.group_by_dynamic)<E: [AsRef](https://doc.rust-lang.org/nightly/core/convert/trait.AsRef.html)<[[Expr](../dsl/enum.Expr.html)]>>(
    self,
    index_column: [Expr](../dsl/enum.Expr.html),
    group_by: E,
    options: [DynamicGroupOptions](../prelude/struct.DynamicGroupOptions.html),
) -> [LazyGroupBy](struct.LazyGroupBy.html)

**crate feature**only.

`dynamic_group_by`
Group based on a time value (or index value of type Int32, Int64).

Time windows are calculated and rows are assigned to windows. Different from a normal group_by is that a row can be member of multiple groups. The time/index window could be seen as a rolling window, with a window size determined by dates/times/values instead of slots in the DataFrame.

A window is defined by:

- every: interval of the window
- period: length of the window
- offset: offset of the window

The `group_by` argument should be empty `[]` if you don’t want to combine this
with a ordinary group_by on these keys.

## [Source](../../src/polars_lazy/frame/mod.rs.html#1167-1198)
#### pub fn [group_by_stable](#method.group_by_stable)<E: [AsRef](https://doc.rust-lang.org/nightly/core/convert/trait.AsRef.html)<[\[IE\]](https://doc.rust-lang.org/nightly/std/primitive.slice.html)>, IE: [Into](https://doc.rust-lang.org/nightly/core/convert/trait.Into.html)<[Expr](../dsl/enum.Expr.html)> + [Clone](https://doc.rust-lang.org/nightly/core/clone/trait.Clone.html)>(
    self,
    by: E,
) -> [LazyGroupBy](struct.LazyGroupBy.html)

[Source](../../src/polars_lazy/frame/mod.rs.html#1167-1198)

#### pub fn [group_by_stable](#method.group_by_stable)<E: [AsRef](https://doc.rust-lang.org/nightly/core/convert/trait.AsRef.html)<[\[IE\]](https://doc.rust-lang.org/nightly/std/primitive.slice.html)>, IE: [Into](https://doc.rust-lang.org/nightly/core/convert/trait.Into.html)<[Expr](../dsl/enum.Expr.html)> + [Clone](https://doc.rust-lang.org/nightly/core/clone/trait.Clone.html)>(
    self,
    by: E,
) -> [LazyGroupBy](struct.LazyGroupBy.html)

Similar to [`group_by`](struct.LazyFrame.html#method.group_by), but order of the DataFrame is maintained.

## [Source](../../src/polars_lazy/frame/mod.rs.html#1217-1224)
#### pub fn [anti_join](#method.anti_join)<E: [Into](https://doc.rust-lang.org/nightly/core/convert/trait.Into.html)<[Expr](../dsl/enum.Expr.html)>>(
    self,
    other: [LazyFrame](struct.LazyFrame.html),
    left_on: E,
    right_on: E,
) -> [LazyFrame](struct.LazyFrame.html)

**crate feature `semi_anti_join`** only.

[Source](../../src/polars_lazy/frame/mod.rs.html#1217-1224)

#### pub fn [anti_join](#method.anti_join)<E: [Into](https://doc.rust-lang.org/nightly/core/convert/trait.Into.html)<[Expr](../dsl/enum.Expr.html)>>(
    self,
    other: [LazyFrame](struct.LazyFrame.html),
    left_on: E,
    right_on: E,
) -> [LazyFrame](struct.LazyFrame.html)

**crate feature**only.

`semi_anti_join`
Left anti join this query with another lazy query.

Matches on the values of the expressions `left_on` and `right_on`. For more
flexible join logic, see [`join`](struct.LazyFrame.html#method.join) or
[`join_builder`](struct.LazyFrame.html#method.join_builder).

#####Example

```
use polars_core::prelude::*;
use polars_lazy::prelude::*;
fn anti_join_dataframes(ldf: LazyFrame, other: LazyFrame) -> LazyFrame {
        ldf
        .anti_join(other, col("foo"), col("bar").cast(DataType::String))
}
```
## [Source](../../src/polars_lazy/frame/mod.rs.html#1228-1235)
#### pub fn [cross_join](#method.cross_join)(
    self,
    other: [LazyFrame](struct.LazyFrame.html),
    suffix: [Option](https://doc.rust-lang.org/nightly/core/option/enum.Option.html)<[PlSmallStr](../../polars_utils/pl_str/struct.PlSmallStr.html)>,
) -> [LazyFrame](struct.LazyFrame.html)

**crate feature `cross_join`** only.

[Source](../../src/polars_lazy/frame/mod.rs.html#1228-1235)

#### pub fn [cross_join](#method.cross_join)(
    self,
    other: [LazyFrame](struct.LazyFrame.html),
    suffix: [Option](https://doc.rust-lang.org/nightly/core/option/enum.Option.html)<[PlSmallStr](../../polars_utils/pl_str/struct.PlSmallStr.html)>,
) -> [LazyFrame](struct.LazyFrame.html)

**crate feature**only.

`cross_join`
Creates the Cartesian product from both frames, preserving the order of the left keys.

## [Source](../../src/polars_lazy/frame/mod.rs.html#1253-1260)
#### pub fn [left_join](#method.left_join)<E: [Into](https://doc.rust-lang.org/nightly/core/convert/trait.Into.html)<[Expr](../dsl/enum.Expr.html)>>(
    self,
    other: [LazyFrame](struct.LazyFrame.html),
    left_on: E,
    right_on: E,
) -> [LazyFrame](struct.LazyFrame.html)

[Source](../../src/polars_lazy/frame/mod.rs.html#1253-1260)

#### pub fn [left_join](#method.left_join)<E: [Into](https://doc.rust-lang.org/nightly/core/convert/trait.Into.html)<[Expr](../dsl/enum.Expr.html)>>(
    self,
    other: [LazyFrame](struct.LazyFrame.html),
    left_on: E,
    right_on: E,
) -> [LazyFrame](struct.LazyFrame.html)

Left outer join this query with another lazy query.

Matches on the values of the expressions `left_on` and `right_on`. For more
flexible join logic, see [`join`](struct.LazyFrame.html#method.join) or
[`join_builder`](struct.LazyFrame.html#method.join_builder).

#####Example

```
use polars_core::prelude::*;
use polars_lazy::prelude::*;
fn left_join_dataframes(ldf: LazyFrame, other: LazyFrame) -> LazyFrame {
        ldf
        .left_join(other, col("foo"), col("bar"))
}
```
## [Source](../../src/polars_lazy/frame/mod.rs.html#1278-1285)
#### pub fn [inner_join](#method.inner_join)<E: [Into](https://doc.rust-lang.org/nightly/core/convert/trait.Into.html)<[Expr](../dsl/enum.Expr.html)>>(
    self,
    other: [LazyFrame](struct.LazyFrame.html),
    left_on: E,
    right_on: E,
) -> [LazyFrame](struct.LazyFrame.html)

[Source](../../src/polars_lazy/frame/mod.rs.html#1278-1285)

#### pub fn [inner_join](#method.inner_join)<E: [Into](https://doc.rust-lang.org/nightly/core/convert/trait.Into.html)<[Expr](../dsl/enum.Expr.html)>>(
    self,
    other: [LazyFrame](struct.LazyFrame.html),
    left_on: E,
    right_on: E,
) -> [LazyFrame](struct.LazyFrame.html)

Inner join this query with another lazy query.

Matches on the values of the expressions `left_on` and `right_on`. For more
flexible join logic, see [`join`](struct.LazyFrame.html#method.join) or
[`join_builder`](struct.LazyFrame.html#method.join_builder).

#####Example

```
use polars_core::prelude::*;
use polars_lazy::prelude::*;
fn inner_join_dataframes(ldf: LazyFrame, other: LazyFrame) -> LazyFrame {
        ldf
        .inner_join(other, col("foo"), col("bar").cast(DataType::String))
}
```
## [Source](../../src/polars_lazy/frame/mod.rs.html#1303-1310)
#### pub fn [full_join](#method.full_join)<E: [Into](https://doc.rust-lang.org/nightly/core/convert/trait.Into.html)<[Expr](../dsl/enum.Expr.html)>>(
    self,
    other: [LazyFrame](struct.LazyFrame.html),
    left_on: E,
    right_on: E,
) -> [LazyFrame](struct.LazyFrame.html)

[Source](../../src/polars_lazy/frame/mod.rs.html#1303-1310)

#### pub fn [full_join](#method.full_join)<E: [Into](https://doc.rust-lang.org/nightly/core/convert/trait.Into.html)<[Expr](../dsl/enum.Expr.html)>>(
    self,
    other: [LazyFrame](struct.LazyFrame.html),
    left_on: E,
    right_on: E,
) -> [LazyFrame](struct.LazyFrame.html)

Full outer join this query with another lazy query.

Matches on the values of the expressions `left_on` and `right_on`. For more
flexible join logic, see [`join`](struct.LazyFrame.html#method.join) or
[`join_builder`](struct.LazyFrame.html#method.join_builder).

#####Example

```
use polars_core::prelude::*;
use polars_lazy::prelude::*;
fn full_join_dataframes(ldf: LazyFrame, other: LazyFrame) -> LazyFrame {
        ldf
        .full_join(other, col("foo"), col("bar"))
}
```
## [Source](../../src/polars_lazy/frame/mod.rs.html#1329-1336)
#### pub fn [semi_join](#method.semi_join)<E: [Into](https://doc.rust-lang.org/nightly/core/convert/trait.Into.html)<[Expr](../dsl/enum.Expr.html)>>(
    self,
    other: [LazyFrame](struct.LazyFrame.html),
    left_on: E,
    right_on: E,
) -> [LazyFrame](struct.LazyFrame.html)

**crate feature `semi_anti_join`** only.

[Source](../../src/polars_lazy/frame/mod.rs.html#1329-1336)

#### pub fn [semi_join](#method.semi_join)<E: [Into](https://doc.rust-lang.org/nightly/core/convert/trait.Into.html)<[Expr](../dsl/enum.Expr.html)>>(
    self,
    other: [LazyFrame](struct.LazyFrame.html),
    left_on: E,
    right_on: E,
) -> [LazyFrame](struct.LazyFrame.html)

**crate feature**only.

`semi_anti_join`
Left semi join this query with another lazy query.

Matches on the values of the expressions `left_on` and `right_on`. For more
flexible join logic, see [`join`](struct.LazyFrame.html#method.join) or
[`join_builder`](struct.LazyFrame.html#method.join_builder).

#####Example

```
use polars_core::prelude::*;
use polars_lazy::prelude::*;
fn semi_join_dataframes(ldf: LazyFrame, other: LazyFrame) -> LazyFrame {
        ldf
        .semi_join(other, col("foo"), col("bar").cast(DataType::String))
}
```
## [Source](../../src/polars_lazy/frame/mod.rs.html#1359-1370)
#### pub fn [join](#method.join)<E: [AsRef](https://doc.rust-lang.org/nightly/core/convert/trait.AsRef.html)<[[Expr](../dsl/enum.Expr.html)]>>(
    self,
    other: [LazyFrame](struct.LazyFrame.html),
    left_on: E,
    right_on: E,
    args: [JoinArgs](../prelude/struct.JoinArgs.html),
) -> [LazyFrame](struct.LazyFrame.html)

[Source](../../src/polars_lazy/frame/mod.rs.html#1359-1370)

#### pub fn [join](#method.join)<E: [AsRef](https://doc.rust-lang.org/nightly/core/convert/trait.AsRef.html)<[[Expr](../dsl/enum.Expr.html)]>>(
    self,
    other: [LazyFrame](struct.LazyFrame.html),
    left_on: E,
    right_on: E,
    args: [JoinArgs](../prelude/struct.JoinArgs.html),
) -> [LazyFrame](struct.LazyFrame.html)

Generic function to join two LazyFrames.

`join` can join on multiple columns, given as two list of expressions, and with a
[`JoinType`](../prelude/enum.JoinType.html) specified by `how`. Non-joined column names in the right DataFrame
that already exist in this DataFrame are suffixed with `"_right"`. For control
over how columns are renamed and parallelization options, use
[`join_builder`](struct.LazyFrame.html#method.join_builder).

Any provided `args.slice` parameter is not considered, but set by the internal optimizer.

#####Example

```
use polars_core::prelude::*;
use polars_lazy::prelude::*;
fn example(ldf: LazyFrame, other: LazyFrame) -> LazyFrame {
        ldf
        .join(other, [col("foo"), col("bar")], [col("foo"), col("bar")], JoinArgs::new(JoinType::Inner))
}
```
## [Source](../../src/polars_lazy/frame/mod.rs.html#1419-1421)
#### pub fn [join_builder](#method.join_builder)(self) -> [JoinBuilder](struct.JoinBuilder.html)

[Source](../../src/polars_lazy/frame/mod.rs.html#1419-1421)

#### pub fn [join_builder](#method.join_builder)(self) -> [JoinBuilder](struct.JoinBuilder.html)

Consume `self` and return a [`JoinBuilder`](struct.JoinBuilder.html) to customize a join on this LazyFrame.

After the `JoinBuilder` has been created and set up, calling
[`finish()`](struct.JoinBuilder.html#method.finish) on it will give back the `LazyFrame`
representing the `join` operation.

## [Source](../../src/polars_lazy/frame/mod.rs.html#1426-1433)
#### pub fn [gather](#method.gather)(self, idxs: [LazyFrame](struct.LazyFrame.html), null_on_oob: [bool](https://doc.rust-lang.org/nightly/std/primitive.bool.html)) -> [LazyFrame](struct.LazyFrame.html)

[Source](../../src/polars_lazy/frame/mod.rs.html#1426-1433)

#### pub fn [gather](#method.gather)(self, idxs: [LazyFrame](struct.LazyFrame.html), null_on_oob: [bool](https://doc.rust-lang.org/nightly/std/primitive.bool.html)) -> [LazyFrame](struct.LazyFrame.html)

Gathers rows from this DataFrame based on the indices in idxs.

idxs must only have a single column of indices.

## [Source](../../src/polars_lazy/frame/mod.rs.html#1452-1466)
#### pub fn [with_column](#method.with_column)(self, expr: [Expr](../dsl/enum.Expr.html)) -> [LazyFrame](struct.LazyFrame.html)

[Source](../../src/polars_lazy/frame/mod.rs.html#1452-1466)

#### pub fn [with_column](#method.with_column)(self, expr: [Expr](../dsl/enum.Expr.html)) -> [LazyFrame](struct.LazyFrame.html)

Add or replace a column, given as an expression, to a DataFrame.

#####Example

```
use polars_core::prelude::*;
use polars_lazy::prelude::*;
fn add_column(df: DataFrame) -> LazyFrame {
    df.lazy()
        .with_column(
            when(col("sepal_length").lt(lit(5.0)))
            .then(lit(10))
            .otherwise(lit(1))
            .alias("new_column_name"),
        )
}
```
## [Source](../../src/polars_lazy/frame/mod.rs.html#1482-1492)
#### pub fn [with_columns](#method.with_columns)<E: [AsRef](https://doc.rust-lang.org/nightly/core/convert/trait.AsRef.html)<[[Expr](../dsl/enum.Expr.html)]>>(self, exprs: E) -> [LazyFrame](struct.LazyFrame.html)

[Source](../../src/polars_lazy/frame/mod.rs.html#1482-1492)

#### pub fn [with_columns](#method.with_columns)<E: [AsRef](https://doc.rust-lang.org/nightly/core/convert/trait.AsRef.html)<[[Expr](../dsl/enum.Expr.html)]>>(self, exprs: E) -> [LazyFrame](struct.LazyFrame.html)

Add or replace multiple columns, given as expressions, to a DataFrame.

#####Example

```
use polars_core::prelude::*;
use polars_lazy::prelude::*;
fn add_columns(df: DataFrame) -> LazyFrame {
    df.lazy()
        .with_columns(
            vec![lit(10).alias("foo"), lit(100).alias("bar")]
         )
}
```
## [Source](../../src/polars_lazy/frame/mod.rs.html#1495-1505)
#### pub fn [with_columns_seq](#method.with_columns_seq)<E: [AsRef](https://doc.rust-lang.org/nightly/core/convert/trait.AsRef.html)<[[Expr](../dsl/enum.Expr.html)]>>(self, exprs: E) -> [LazyFrame](struct.LazyFrame.html)

[Source](../../src/polars_lazy/frame/mod.rs.html#1495-1505)

#### pub fn [with_columns_seq](#method.with_columns_seq)<E: [AsRef](https://doc.rust-lang.org/nightly/core/convert/trait.AsRef.html)<[[Expr](../dsl/enum.Expr.html)]>>(self, exprs: E) -> [LazyFrame](struct.LazyFrame.html)

Add or replace multiple columns to a DataFrame, but evaluate them sequentially.

## [Source](../../src/polars_lazy/frame/mod.rs.html#1508-1520)
#### pub fn [match_to_schema](#method.match_to_schema)(
    self,
    schema: [SchemaRef](../../polars_core/schema/type.SchemaRef.html),
    per_column: [Arc](https://doc.rust-lang.org/nightly/alloc/sync/struct.Arc.html)<[[MatchToSchemaPerColumn](../dsl/struct.MatchToSchemaPerColumn.html)]>,
    extra_columns: [ExtraColumnsPolicy](../dsl/enum.ExtraColumnsPolicy.html),
) -> [LazyFrame](struct.LazyFrame.html)

[Source](../../src/polars_lazy/frame/mod.rs.html#1508-1520)

#### pub fn [match_to_schema](#method.match_to_schema)(
    self,
    schema: [SchemaRef](../../polars_core/schema/type.SchemaRef.html),
    per_column: [Arc](https://doc.rust-lang.org/nightly/alloc/sync/struct.Arc.html)<[[MatchToSchemaPerColumn](../dsl/struct.MatchToSchemaPerColumn.html)]>,
    extra_columns: [ExtraColumnsPolicy](../dsl/enum.ExtraColumnsPolicy.html),
) -> [LazyFrame](struct.LazyFrame.html)

Match or evolve to a certain schema.

[Source](../../src/polars_lazy/frame/mod.rs.html#1522-1532)

#### pub fn [pipe_with_schema](#method.pipe_with_schema)(
    self,
    callback: [PlanCallback](../prelude/enum.PlanCallback.html)<([Vec](https://doc.rust-lang.org/nightly/alloc/vec/struct.Vec.html)<[DslPlan](../dsl/enum.DslPlan.html)>, [Vec](https://doc.rust-lang.org/nightly/alloc/vec/struct.Vec.html)<[SchemaRef](../../polars_core/schema/type.SchemaRef.html)>), [DslPlan](../dsl/enum.DslPlan.html)>,
) -> Self

[Source](../../src/polars_lazy/frame/mod.rs.html#1534-1548)

#### pub fn [pipe_with_schemas](#method.pipe_with_schemas)(
    self,
    others: [Vec](https://doc.rust-lang.org/nightly/alloc/vec/struct.Vec.html)<[LazyFrame](struct.LazyFrame.html)>,
    callback: [PlanCallback](../prelude/enum.PlanCallback.html)<([Vec](https://doc.rust-lang.org/nightly/alloc/vec/struct.Vec.html)<[DslPlan](../dsl/enum.DslPlan.html)>, [Vec](https://doc.rust-lang.org/nightly/alloc/vec/struct.Vec.html)<[SchemaRef](../../polars_core/schema/type.SchemaRef.html)>), [DslPlan](../dsl/enum.DslPlan.html)>,
) -> Self

[Source](../../src/polars_lazy/frame/mod.rs.html#1556-1565)

#### pub fn [with_context](#method.with_context)<C: [AsRef](https://doc.rust-lang.org/nightly/core/convert/trait.AsRef.html)<[[LazyFrame](struct.LazyFrame.html)]>>(self, contexts: C) -> [LazyFrame](struct.LazyFrame.html)

## [Source](../../src/polars_lazy/frame/mod.rs.html#1570-1572)
#### pub fn [max](#method.max)(self) -> Self

[Source](../../src/polars_lazy/frame/mod.rs.html#1570-1572)

#### pub fn [max](#method.max)(self) -> Self

Aggregate all the columns as their maximum values.

Aggregated columns will have the same names as the original columns.

## [Source](../../src/polars_lazy/frame/mod.rs.html#1577-1579)
#### pub fn [min](#method.min)(self) -> Self

[Source](../../src/polars_lazy/frame/mod.rs.html#1577-1579)

#### pub fn [min](#method.min)(self) -> Self

Aggregate all the columns as their minimum values.

Aggregated columns will have the same names as the original columns.

## [Source](../../src/polars_lazy/frame/mod.rs.html#1590-1592)
#### pub fn [sum](#method.sum)(self) -> Self

[Source](../../src/polars_lazy/frame/mod.rs.html#1590-1592)

#### pub fn [sum](#method.sum)(self) -> Self

Aggregate all the columns as their sum values.

Aggregated columns will have the same names as the original columns.

- Boolean columns will sum to a `u32` containing the number of`true` s.
- For integer columns, the ordinary checks for overflow are performed:
if running in `debug` mode, overflows will panic, whereas in`release` mode overflows will
silently wrap.
- String columns will sum to None.

## [Source](../../src/polars_lazy/frame/mod.rs.html#1598-1600)
#### pub fn [mean](#method.mean)(self) -> Self

[Source](../../src/polars_lazy/frame/mod.rs.html#1598-1600)

#### pub fn [mean](#method.mean)(self) -> Self

Aggregate all the columns as their mean values.

- Boolean and integer columns are converted to `f64` before computing the mean.
- String columns will have a mean of None.

## [Source](../../src/polars_lazy/frame/mod.rs.html#1607-1609)
#### pub fn [median](#method.median)(self) -> Self

[Source](../../src/polars_lazy/frame/mod.rs.html#1607-1609)

#### pub fn [median](#method.median)(self) -> Self

Aggregate all the columns as their median values.

- Boolean and integer results are converted to `f64` . However, they are still
susceptible to overflow before this conversion occurs.
- String columns will sum to None.

## [Source](../../src/polars_lazy/frame/mod.rs.html#1612-1617)
#### pub fn [quantile](#method.quantile)(self, quantile: [Expr](../dsl/enum.Expr.html), method: QuantileMethod) -> Self

[Source](../../src/polars_lazy/frame/mod.rs.html#1612-1617)

#### pub fn [quantile](#method.quantile)(self, quantile: [Expr](../dsl/enum.Expr.html), method: QuantileMethod) -> Self

Aggregate all the columns as their quantile values.

## [Source](../../src/polars_lazy/frame/mod.rs.html#1631-1633)
#### pub fn [std](#method.std)(self, ddof: [u8](https://doc.rust-lang.org/nightly/std/primitive.u8.html)) -> Self

[Source](../../src/polars_lazy/frame/mod.rs.html#1631-1633)

#### pub fn [std](#method.std)(self, ddof: [u8](https://doc.rust-lang.org/nightly/std/primitive.u8.html)) -> Self

Aggregate all the columns as their standard deviation values.

`ddof` is the “Delta Degrees of Freedom”; `N - ddof` will be the denominator when
computing the variance, where `N` is the number of rows.

In standard statistical practice, `ddof=1` provides an unbiased estimator of the
variance of a hypothetical infinite population. `ddof=0` provides a maximum
likelihood estimate of the variance for normally distributed variables. The
standard deviation computed in this function is the square root of the estimated
variance, so even with `ddof=1`, it will not be an unbiased estimate of the
standard deviation per se.

Source: [Numpy](https://numpy.org/doc/stable/reference/generated/numpy.std.html#)

## [Source](../../src/polars_lazy/frame/mod.rs.html#1644-1646)
#### pub fn [var](#method.var)(self, ddof: [u8](https://doc.rust-lang.org/nightly/std/primitive.u8.html)) -> Self

[Source](../../src/polars_lazy/frame/mod.rs.html#1644-1646)

#### pub fn [var](#method.var)(self, ddof: [u8](https://doc.rust-lang.org/nightly/std/primitive.u8.html)) -> Self

Aggregate all the columns as their variance values.

`ddof` is the “Delta Degrees of Freedom”; `N - ddof` will be the denominator when
computing the variance, where `N` is the number of rows.

In standard statistical practice, `ddof=1` provides an unbiased estimator of the
variance of a hypothetical infinite population. `ddof=0` provides a maximum
likelihood estimate of the variance for normally distributed variables.

Source: [Numpy](https://numpy.org/doc/stable/reference/generated/numpy.var.html#)

## [Source](../../src/polars_lazy/frame/mod.rs.html#1649-1651)
#### pub fn [explode](#method.explode)(self, columns: [Selector](../dsl/enum.Selector.html), options: [ExplodeOptions](../../polars_core/chunked_array/ops/struct.ExplodeOptions.html)) -> [LazyFrame](struct.LazyFrame.html)

[Source](../../src/polars_lazy/frame/mod.rs.html#1649-1651)

#### pub fn [explode](#method.explode)(self, columns: [Selector](../dsl/enum.Selector.html), options: [ExplodeOptions](../../polars_core/chunked_array/ops/struct.ExplodeOptions.html)) -> [LazyFrame](struct.LazyFrame.html)

Apply explode operation. [See eager explode](../../polars_core/frame/dataframe/struct.DataFrame.html#method.explode).

## [Source](../../src/polars_lazy/frame/mod.rs.html#1669-1671)
#### pub fn [null_count](#method.null_count)(self) -> [LazyFrame](struct.LazyFrame.html)

[Source](../../src/polars_lazy/frame/mod.rs.html#1669-1671)

#### pub fn [null_count](#method.null_count)(self) -> [LazyFrame](struct.LazyFrame.html)

Aggregate all the columns as the sum of their null value count.

## [Source](../../src/polars_lazy/frame/mod.rs.html#1677-1684)
#### pub fn [unique_stable](#method.unique_stable)(
    self,
    subset: [Option](https://doc.rust-lang.org/nightly/core/option/enum.Option.html)<[Selector](../dsl/enum.Selector.html)>,
    keep_strategy: [UniqueKeepStrategy](../../polars_core/frame/enum.UniqueKeepStrategy.html),
) -> [LazyFrame](struct.LazyFrame.html)

[Source](../../src/polars_lazy/frame/mod.rs.html#1677-1684)

#### pub fn [unique_stable](#method.unique_stable)(
    self,
    subset: [Option](https://doc.rust-lang.org/nightly/core/option/enum.Option.html)<[Selector](../dsl/enum.Selector.html)>,
    keep_strategy: [UniqueKeepStrategy](../../polars_core/frame/enum.UniqueKeepStrategy.html),
) -> [LazyFrame](struct.LazyFrame.html)

Drop non-unique rows and maintain the order of kept rows.

`subset` is an optional `Vec` of column names to consider for uniqueness; if
`None`, all columns are considered.

[Source](../../src/polars_lazy/frame/mod.rs.html#1686-1699)

#### pub fn [unique_stable_generic](#method.unique_stable_generic)(
    self,
    subset: [Option](https://doc.rust-lang.org/nightly/core/option/enum.Option.html)<[Vec](https://doc.rust-lang.org/nightly/alloc/vec/struct.Vec.html)<[Expr](../dsl/enum.Expr.html)>>,
    keep_strategy: [UniqueKeepStrategy](../../polars_core/frame/enum.UniqueKeepStrategy.html),
) -> [LazyFrame](struct.LazyFrame.html)

## [Source](../../src/polars_lazy/frame/mod.rs.html#1708-1711)
#### pub fn [unique](#method.unique)(
    self,
    subset: [Option](https://doc.rust-lang.org/nightly/core/option/enum.Option.html)<[Selector](../dsl/enum.Selector.html)>,
    keep_strategy: [UniqueKeepStrategy](../../polars_core/frame/enum.UniqueKeepStrategy.html),
) -> [LazyFrame](struct.LazyFrame.html)

[Source](../../src/polars_lazy/frame/mod.rs.html#1708-1711)

#### pub fn [unique](#method.unique)(
    self,
    subset: [Option](https://doc.rust-lang.org/nightly/core/option/enum.Option.html)<[Selector](../dsl/enum.Selector.html)>,
    keep_strategy: [UniqueKeepStrategy](../../polars_core/frame/enum.UniqueKeepStrategy.html),
) -> [LazyFrame](struct.LazyFrame.html)

Drop non-unique rows without maintaining the order of kept rows.

The order of the kept rows may change; to maintain the original row order, use
[`unique_stable`](struct.LazyFrame.html#method.unique_stable).

`subset` is an optional `Vec` of column names to consider for uniqueness; if None,
all columns are considered.

[Source](../../src/polars_lazy/frame/mod.rs.html#1713-1726)

#### pub fn [unique_generic](#method.unique_generic)(
    self,
    subset: [Option](https://doc.rust-lang.org/nightly/core/option/enum.Option.html)<[Vec](https://doc.rust-lang.org/nightly/alloc/vec/struct.Vec.html)<[Expr](../dsl/enum.Expr.html)>>,
    keep_strategy: [UniqueKeepStrategy](../../polars_core/frame/enum.UniqueKeepStrategy.html),
) -> [LazyFrame](struct.LazyFrame.html)

## [Source](../../src/polars_lazy/frame/mod.rs.html#1732-1736)
#### pub fn [drop_nans](#method.drop_nans)(self, subset: [Option](https://doc.rust-lang.org/nightly/core/option/enum.Option.html)<[Selector](../dsl/enum.Selector.html)>) -> [LazyFrame](struct.LazyFrame.html)

[Source](../../src/polars_lazy/frame/mod.rs.html#1732-1736)

#### pub fn [drop_nans](#method.drop_nans)(self, subset: [Option](https://doc.rust-lang.org/nightly/core/option/enum.Option.html)<[Selector](../dsl/enum.Selector.html)>) -> [LazyFrame](struct.LazyFrame.html)

Drop rows containing one or more NaN values.

`subset` is an optional `Vec` of column names to consider for NaNs; if None, all
floating point columns are considered.

## [Source](../../src/polars_lazy/frame/mod.rs.html#1742-1746)
#### pub fn [drop_nulls](#method.drop_nulls)(self, subset: [Option](https://doc.rust-lang.org/nightly/core/option/enum.Option.html)<[Selector](../dsl/enum.Selector.html)>) -> [LazyFrame](struct.LazyFrame.html)

[Source](../../src/polars_lazy/frame/mod.rs.html#1742-1746)

#### pub fn [drop_nulls](#method.drop_nulls)(self, subset: [Option](https://doc.rust-lang.org/nightly/core/option/enum.Option.html)<[Selector](../dsl/enum.Selector.html)>) -> [LazyFrame](struct.LazyFrame.html)

Drop rows containing one or more None values.

`subset` is an optional `Vec` of column names to consider for nulls; if None, all
columns are considered.

## [Source](../../src/polars_lazy/frame/mod.rs.html#1757-1761)
#### pub fn [slice](#method.slice)(self, offset: [i64](https://doc.rust-lang.org/nightly/std/primitive.i64.html), len: [IdxSize](../../polars_utils/index/type.IdxSize.html)) -> [LazyFrame](struct.LazyFrame.html)

[Source](../../src/polars_lazy/frame/mod.rs.html#1757-1761)

#### pub fn [slice](#method.slice)(self, offset: [i64](https://doc.rust-lang.org/nightly/std/primitive.i64.html), len: [IdxSize](../../polars_utils/index/type.IdxSize.html)) -> [LazyFrame](struct.LazyFrame.html)

Slice the DataFrame using an offset (starting row) and a length.

If `offset` is negative, it is counted from the end of the DataFrame. For
instance, `lf.slice(-5, 3)` gets three rows, starting at the row fifth from the
end.

If `offset` and `len` are such that the slice extends beyond the end of the
DataFrame, the portion between `offset` and the end will be returned. In this
case, the number of rows in the returned DataFrame will be less than `len`.

## [Source](../../src/polars_lazy/frame/mod.rs.html#1785-1788)
#### pub fn [tail](#method.tail)(self, n: [IdxSize](../../polars_utils/index/type.IdxSize.html)) -> [LazyFrame](struct.LazyFrame.html)

[Source](../../src/polars_lazy/frame/mod.rs.html#1785-1788)

#### pub fn [tail](#method.tail)(self, n: [IdxSize](../../polars_utils/index/type.IdxSize.html)) -> [LazyFrame](struct.LazyFrame.html)

Get the last `n` rows.

Equivalent to `self.slice(-(n as i64), n)`.

[Source](../../src/polars_lazy/frame/mod.rs.html#1792-1818)

#### pub fn [pivot](#method.pivot)(
    self,
    on: [Selector](../dsl/enum.Selector.html),
    on_columns: [Arc](https://doc.rust-lang.org/nightly/alloc/sync/struct.Arc.html)<[DataFrame](../../polars_core/frame/dataframe/struct.DataFrame.html)>,
    index: [Selector](../dsl/enum.Selector.html),
    values: [Selector](../dsl/enum.Selector.html),
    agg: [Expr](../dsl/enum.Expr.html),
    maintain_order: [bool](https://doc.rust-lang.org/nightly/std/primitive.bool.html),
    separator: [PlSmallStr](../../polars_utils/pl_str/struct.PlSmallStr.html),
    column_naming: [PivotColumnNaming](../../polars_core/frame/enum.PivotColumnNaming.html),
) -> [LazyFrame](struct.LazyFrame.html)

**crate feature**only.

`pivot`
## [Source](../../src/polars_lazy/frame/mod.rs.html#1824-1828)
#### pub fn [unpivot](#method.unpivot)(self, args: [UnpivotArgsDSL](../dsl/struct.UnpivotArgsDSL.html)) -> [LazyFrame](struct.LazyFrame.html)

**crate feature `pivot`** only.

[Source](../../src/polars_lazy/frame/mod.rs.html#1824-1828)

#### pub fn [unpivot](#method.unpivot)(self, args: [UnpivotArgsDSL](../dsl/struct.UnpivotArgsDSL.html)) -> [LazyFrame](struct.LazyFrame.html)

**crate feature**only.

`pivot`
Unpivot the DataFrame from wide to long format.

See [`UnpivotArgsIR`](../../polars_core/frame/explode/struct.UnpivotArgsIR.html) for information on how to unpivot a DataFrame.

## [Source](../../src/polars_lazy/frame/mod.rs.html#1848-1869)
#### pub fn [map](#method.map)<F>(
    self,
    function: F,
    optimizations: [AllowedOptimizations](type.AllowedOptimizations.html),
    schema: [Option](https://doc.rust-lang.org/nightly/core/option/enum.Option.html)<[Arc](https://doc.rust-lang.org/nightly/alloc/sync/struct.Arc.html)<dyn [UdfSchema](../dsl/trait.UdfSchema.html)>>,
    name: [Option](https://doc.rust-lang.org/nightly/core/option/enum.Option.html)<&'static [str](https://doc.rust-lang.org/nightly/std/primitive.str.html)>,
) -> [LazyFrame](struct.LazyFrame.html)

[Source](../../src/polars_lazy/frame/mod.rs.html#1848-1869)

#### pub fn [map](#method.map)<F>(
    self,
    function: F,
    optimizations: [AllowedOptimizations](type.AllowedOptimizations.html),
    schema: [Option](https://doc.rust-lang.org/nightly/core/option/enum.Option.html)<[Arc](https://doc.rust-lang.org/nightly/alloc/sync/struct.Arc.html)<dyn [UdfSchema](../dsl/trait.UdfSchema.html)>>,
    name: [Option](https://doc.rust-lang.org/nightly/core/option/enum.Option.html)<&'static [str](https://doc.rust-lang.org/nightly/std/primitive.str.html)>,
) -> [LazyFrame](struct.LazyFrame.html)

Apply a function/closure once the logical plan get executed.

The function has access to the whole materialized DataFrame at the time it is called.

To apply specific functions to specific columns, use [`Expr::map`](../dsl/enum.Expr.html#method.map) in conjunction
with `LazyFrame::with_column` or `with_columns`.

######Warning

This can blow up in your face if the schema is changed due to the operation. The optimizer relies on a correct schema.

You can toggle certain optimizations off.

[Source](../../src/polars_lazy/frame/mod.rs.html#1872-1885)

#### pub fn [map_python](#method.map_python)(
    self,
    function: [PythonFunction](../dsl/python_dsl/type.PythonFunction.html),
    optimizations: [AllowedOptimizations](type.AllowedOptimizations.html),
    schema: [Option](https://doc.rust-lang.org/nightly/core/option/enum.Option.html)<[SchemaRef](../../polars_core/schema/type.SchemaRef.html)>,
    validate_output: [bool](https://doc.rust-lang.org/nightly/std/primitive.bool.html),
) -> [LazyFrame](struct.LazyFrame.html)

**crate feature**only.

`python`
## [Source](../../src/polars_lazy/frame/mod.rs.html#1901-1943)
#### pub fn [with_row_index](#method.with_row_index)<S>(self, name: S, offset: [Option](https://doc.rust-lang.org/nightly/core/option/enum.Option.html)<[IdxSize](../../polars_utils/index/type.IdxSize.html)>) -> [LazyFrame](struct.LazyFrame.html)where
    S: [Into](https://doc.rust-lang.org/nightly/core/convert/trait.Into.html)<[PlSmallStr](../../polars_utils/pl_str/struct.PlSmallStr.html)>,

[Source](../../src/polars_lazy/frame/mod.rs.html#1901-1943)

#### pub fn [with_row_index](#method.with_row_index)<S>(self, name: S, offset: [Option](https://doc.rust-lang.org/nightly/core/option/enum.Option.html)<[IdxSize](../../polars_utils/index/type.IdxSize.html)>) -> [LazyFrame](struct.LazyFrame.html)where
    S: [Into](https://doc.rust-lang.org/nightly/core/convert/trait.Into.html)<[PlSmallStr](../../polars_utils/pl_str/struct.PlSmallStr.html)>,

[Into](https://doc.rust-lang.org/nightly/core/convert/trait.Into.html)<

[PlSmallStr](../../polars_utils/pl_str/struct.PlSmallStr.html)>,

Add a new column at index 0 that counts the rows.

`name` is the name of the new column. `offset` is where to start counting from; if
`None`, it is set to `0`.

#####Warning

This can have a negative effect on query performance. This may for instance block predicate pushdown optimization.

## [Source](../../src/polars_lazy/frame/mod.rs.html#1953-1958)
#### pub fn [unnest](#method.unnest)(self, cols: [Selector](../dsl/enum.Selector.html), separator: [Option](https://doc.rust-lang.org/nightly/core/option/enum.Option.html)<[PlSmallStr](../../polars_utils/pl_str/struct.PlSmallStr.html)>) -> Self

**crate feature `dtype-struct`** only.

[Source](../../src/polars_lazy/frame/mod.rs.html#1953-1958)

#### pub fn [unnest](#method.unnest)(self, cols: [Selector](../dsl/enum.Selector.html), separator: [Option](https://doc.rust-lang.org/nightly/core/option/enum.Option.html)<[PlSmallStr](../../polars_utils/pl_str/struct.PlSmallStr.html)>) -> Self

**crate feature**only.

`dtype-struct`
Unnest the given `Struct` columns: the fields of the `Struct` type will be
inserted as columns.

[Source](../../src/polars_lazy/frame/mod.rs.html#1961-1985)

#### pub fn [merge_sorted](#method.merge_sorted)<I, S>(
    self,
    other: [LazyFrame](struct.LazyFrame.html),
    key: I,
    maintain_order: [bool](https://doc.rust-lang.org/nightly/std/primitive.bool.html),
) -> PolarsResult<[LazyFrame](struct.LazyFrame.html)>

**crate feature**only.

`merge_sorted`
[Source](../../src/polars_lazy/frame/mod.rs.html#1987-1993)

#### pub fn [hint](#method.hint)(self, hint: HintIR) -> PolarsResult<[LazyFrame](struct.LazyFrame.html)>

## [Source](../../src/polars_lazy/scan/anonymous_scan.rs.html#29-76)
### impl [LazyFrame](struct.LazyFrame.html)

[Source](../../src/polars_lazy/scan/anonymous_scan.rs.html#29-76)

### impl [LazyFrame](struct.LazyFrame.html)

[Source](../../src/polars_lazy/scan/anonymous_scan.rs.html#30-75)

#### pub fn [anonymous_scan](#method.anonymous_scan)(
    function: [Arc](https://doc.rust-lang.org/nightly/alloc/sync/struct.Arc.html)<dyn [AnonymousScan](../prelude/trait.AnonymousScan.html)>,
    args: [ScanArgsAnonymous](struct.ScanArgsAnonymous.html),
) -> PolarsResult<Self>

## [Source](../../src/polars_lazy/scan/ipc.rs.html#8-33)
### impl [LazyFrame](struct.LazyFrame.html)

[Source](../../src/polars_lazy/scan/ipc.rs.html#8-33)

### impl [LazyFrame](struct.LazyFrame.html)

## [Source](../../src/polars_lazy/scan/ipc.rs.html#10-20)
#### pub fn [scan_ipc](#method.scan_ipc)(
    path: [PlRefPath](../../polars_utils/pl_path/struct.PlRefPath.html),
    options: [IpcScanOptions](../../polars_io/ipc/ipc_file/struct.IpcScanOptions.html),
    unified_scan_args: [UnifiedScanArgs](../dsl/struct.UnifiedScanArgs.html),
) -> PolarsResult<Self>

**crate feature `ipc`** only.

[Source](../../src/polars_lazy/scan/ipc.rs.html#10-20)

#### pub fn [scan_ipc](#method.scan_ipc)(
    path: [PlRefPath](../../polars_utils/pl_path/struct.PlRefPath.html),
    options: [IpcScanOptions](../../polars_io/ipc/ipc_file/struct.IpcScanOptions.html),
    unified_scan_args: [UnifiedScanArgs](../dsl/struct.UnifiedScanArgs.html),
) -> PolarsResult<Self>

**crate feature**only.

`ipc`
Create a LazyFrame directly from a ipc scan.

[Source](../../src/polars_lazy/scan/ipc.rs.html#22-32)

#### pub fn [scan_ipc_sources](#method.scan_ipc_sources)(
    sources: [ScanSources](../dsl/enum.ScanSources.html),
    options: [IpcScanOptions](../../polars_io/ipc/ipc_file/struct.IpcScanOptions.html),
    unified_scan_args: [UnifiedScanArgs](../dsl/struct.UnifiedScanArgs.html),
) -> PolarsResult<Self>

**crate feature**only.

`ipc`
## [Source](../../src/polars_lazy/scan/parquet.rs.html#167-185)
### impl [LazyFrame](struct.LazyFrame.html)

[Source](../../src/polars_lazy/scan/parquet.rs.html#167-185)

### impl [LazyFrame](struct.LazyFrame.html)

## [Source](../../src/polars_lazy/scan/parquet.rs.html#169-171)
#### pub fn [scan_parquet](#method.scan_parquet)(
    path: [PlRefPath](../../polars_utils/pl_path/struct.PlRefPath.html),
    args: [ScanArgsParquet](struct.ScanArgsParquet.html),
) -> PolarsResult<Self>

**crate feature `parquet`** only.

[Source](../../src/polars_lazy/scan/parquet.rs.html#169-171)

#### pub fn [scan_parquet](#method.scan_parquet)(
    path: [PlRefPath](../../polars_utils/pl_path/struct.PlRefPath.html),
    args: [ScanArgsParquet](struct.ScanArgsParquet.html),
) -> PolarsResult<Self>

**crate feature**only.

`parquet`
Create a LazyFrame directly from a parquet scan.

## [Source](../../src/polars_lazy/scan/parquet.rs.html#174-176)
#### pub fn [scan_parquet_sources](#method.scan_parquet_sources)(
    sources: [ScanSources](../dsl/enum.ScanSources.html),
    args: [ScanArgsParquet](struct.ScanArgsParquet.html),
) -> PolarsResult<Self>

**crate feature `parquet`** only.

[Source](../../src/polars_lazy/scan/parquet.rs.html#174-176)

#### pub fn [scan_parquet_sources](#method.scan_parquet_sources)(
    sources: [ScanSources](../dsl/enum.ScanSources.html),
    args: [ScanArgsParquet](struct.ScanArgsParquet.html),
) -> PolarsResult<Self>

**crate feature**only.

`parquet`
Create a LazyFrame directly from a parquet scan.

## [Source](../../src/polars_lazy/scan/parquet.rs.html#179-184)
#### pub fn [scan_parquet_files](#method.scan_parquet_files)(
    paths: Buffer<[PlRefPath](../../polars_utils/pl_path/struct.PlRefPath.html)>,
    args: [ScanArgsParquet](struct.ScanArgsParquet.html),
) -> PolarsResult<Self>

**crate feature `parquet`** only.

[Source](../../src/polars_lazy/scan/parquet.rs.html#179-184)

#### pub fn [scan_parquet_files](#method.scan_parquet_files)(
    paths: Buffer<[PlRefPath](../../polars_utils/pl_path/struct.PlRefPath.html)>,
    args: [ScanArgsParquet](struct.ScanArgsParquet.html),
) -> PolarsResult<Self>

**crate feature**only.

`parquet`
Create a LazyFrame directly from a parquet scan.

## [Source](../../src/polars_lazy/scan/catalog.rs.html#9-57)
### impl [LazyFrame](struct.LazyFrame.html)

[Source](../../src/polars_lazy/scan/catalog.rs.html#9-57)

### impl [LazyFrame](struct.LazyFrame.html)

[Source](../../src/polars_lazy/scan/catalog.rs.html#10-56)

#### pub fn [scan_catalog_table](#method.scan_catalog_table)(
    table_info: &[TableInfo](../../polars_io/catalog/unity/models/struct.TableInfo.html),
    cloud_options: [Option](https://doc.rust-lang.org/nightly/core/option/enum.Option.html)<[CloudOptions](../../polars_io/cloud/options/struct.CloudOptions.html)>,
) -> PolarsResult<Self>

**crate feature**only.

`catalog`
## Trait Implementations

## [Source](../../src/polars_lazy/frame/mod.rs.html#2010-2018)
### impl [From](https://doc.rust-lang.org/nightly/core/convert/trait.From.html)<[LazyGroupBy](struct.LazyGroupBy.html)> for [LazyFrame](struct.LazyFrame.html)

[Source](../../src/polars_lazy/frame/mod.rs.html#2010-2018)

### impl [From](https://doc.rust-lang.org/nightly/core/convert/trait.From.html)<[LazyGroupBy](struct.LazyGroupBy.html)> for [LazyFrame](struct.LazyFrame.html)

## [Source](../../src/polars_lazy/frame/mod.rs.html#2011-2017)
#### fn [from](https://doc.rust-lang.org/nightly/core/convert/trait.From.html#tymethod.from)(lgb: [LazyGroupBy](struct.LazyGroupBy.html)) -> Self

[Source](../../src/polars_lazy/frame/mod.rs.html#2011-2017)

#### fn [from](https://doc.rust-lang.org/nightly/core/convert/trait.From.html#tymethod.from)(lgb: [LazyGroupBy](struct.LazyGroupBy.html)) -> Self

## Auto Trait Implementations

### impl ![Freeze](https://doc.rust-lang.org/nightly/core/marker/trait.Freeze.html) for [LazyFrame](struct.LazyFrame.html)

### impl ![RefUnwindSafe](https://doc.rust-lang.org/nightly/core/panic/unwind_safe/trait.RefUnwindSafe.html) for [LazyFrame](struct.LazyFrame.html)

### impl [Send](https://doc.rust-lang.org/nightly/core/marker/trait.Send.html) for [LazyFrame](struct.LazyFrame.html)

### impl [Sync](https://doc.rust-lang.org/nightly/core/marker/trait.Sync.html) for [LazyFrame](struct.LazyFrame.html)

### impl [Unpin](https://doc.rust-lang.org/nightly/core/marker/trait.Unpin.html) for [LazyFrame](struct.LazyFrame.html)

### impl [UnsafeUnpin](https://doc.rust-lang.org/nightly/core/marker/trait.UnsafeUnpin.html) for [LazyFrame](struct.LazyFrame.html)

### impl ![UnwindSafe](https://doc.rust-lang.org/nightly/core/panic/unwind_safe/trait.UnwindSafe.html) for [LazyFrame](struct.LazyFrame.html)

## Blanket Implementations

## [Source](https://doc.rust-lang.org/nightly/src/core/borrow.rs.html#221)
### impl<T> [BorrowMut](https://doc.rust-lang.org/nightly/core/borrow/trait.BorrowMut.html)<T> for Twhere
    T: ?[Sized](https://doc.rust-lang.org/nightly/core/marker/trait.Sized.html),

[Source](https://doc.rust-lang.org/nightly/src/core/borrow.rs.html#221)

### impl<T> [BorrowMut](https://doc.rust-lang.org/nightly/core/borrow/trait.BorrowMut.html)<T> for Twhere
    T: ?[Sized](https://doc.rust-lang.org/nightly/core/marker/trait.Sized.html),

[Sized](https://doc.rust-lang.org/nightly/core/marker/trait.Sized.html),

## [Source](https://doc.rust-lang.org/nightly/src/core/borrow.rs.html#222)
#### fn [borrow_mut](https://doc.rust-lang.org/nightly/core/borrow/trait.BorrowMut.html#tymethod.borrow_mut)(&mut self) -> [&mut T](https://doc.rust-lang.org/nightly/std/primitive.reference.html)

[Source](https://doc.rust-lang.org/nightly/src/core/borrow.rs.html#222)

#### fn [borrow_mut](https://doc.rust-lang.org/nightly/core/borrow/trait.BorrowMut.html#tymethod.borrow_mut)(&mut self) -> [&mut T](https://doc.rust-lang.org/nightly/std/primitive.reference.html)

[Read more](https://doc.rust-lang.org/nightly/core/borrow/trait.BorrowMut.html#tymethod.borrow_mut)

## [Source](https://doc.rust-lang.org/nightly/src/core/clone.rs.html#547)
### impl<T> [CloneToUninit](https://doc.rust-lang.org/nightly/core/clone/trait.CloneToUninit.html) for Twhere
    T: [Clone](https://doc.rust-lang.org/nightly/core/clone/trait.Clone.html),

[Source](https://doc.rust-lang.org/nightly/src/core/clone.rs.html#547)

### impl<T> [CloneToUninit](https://doc.rust-lang.org/nightly/core/clone/trait.CloneToUninit.html) for Twhere
    T: [Clone](https://doc.rust-lang.org/nightly/core/clone/trait.Clone.html),

[Clone](https://doc.rust-lang.org/nightly/core/clone/trait.Clone.html),

##
### impl<T> Instrument for T

### impl<T> Instrument for T

##
#### fn [instrument](self, span: Span) -> Instrumented<Self>

#### fn [instrument](self, span: Span) -> Instrumented<Self>

##
#### fn [in_current_span](self) -> Instrumented<Self>

#### fn [in_current_span](self) -> Instrumented<Self>

## [Source](https://docs.rs/either/1/src/either/into_either.rs.html#64)
### impl<T> [IntoEither](https://docs.rs/either/1/either/into_either/trait.IntoEither.html) for T

[Source](https://docs.rs/either/1/src/either/into_either.rs.html#64)

### impl<T> [IntoEither](https://docs.rs/either/1/either/into_either/trait.IntoEither.html) for T

## [Source](https://docs.rs/either/1/src/either/into_either.rs.html#29)
#### fn [into_either](https://docs.rs/either/1/either/into_either/trait.IntoEither.html#method.into_either)(self, into_left: [bool](https://doc.rust-lang.org/nightly/std/primitive.bool.html)) -> [Either](https://docs.rs/either/1/either/enum.Either.html)<Self, Self> [ⓘ](#)

[Source](https://docs.rs/either/1/src/either/into_either.rs.html#29)

#### fn [into_either](https://docs.rs/either/1/either/into_either/trait.IntoEither.html#method.into_either)(self, into_left: [bool](https://doc.rust-lang.org/nightly/std/primitive.bool.html)) -> [Either](https://docs.rs/either/1/either/enum.Either.html)<Self, Self> [ⓘ](#)

`self` into a [variant of](https://docs.rs/either/1/either/enum.Either.html#variant.Left)

`Left`
[if](https://docs.rs/either/1/either/enum.Either.html)

`Either<Self, Self>``into_left` is `true`.
Converts `self` into a [variant of](https://docs.rs/either/1/either/enum.Either.html#variant.Right)

`Right`
[otherwise.](https://docs.rs/either/1/either/enum.Either.html)

`Either<Self, Self>`
[Read more](https://docs.rs/either/1/either/into_either/trait.IntoEither.html#method.into_either)

## [Source](https://docs.rs/either/1/src/either/into_either.rs.html#55-57)
#### fn [into_either_with](https://docs.rs/either/1/either/into_either/trait.IntoEither.html#method.into_either_with)<F>(self, into_left: F) -> [Either](https://docs.rs/either/1/either/enum.Either.html)<Self, Self> [ⓘ](#)

[Source](https://docs.rs/either/1/src/either/into_either.rs.html#55-57)

#### fn [into_either_with](https://docs.rs/either/1/either/into_either/trait.IntoEither.html#method.into_either_with)<F>(self, into_left: F) -> [Either](https://docs.rs/either/1/either/enum.Either.html)<Self, Self> [ⓘ](#)

`self` into a [variant of](https://docs.rs/either/1/either/enum.Either.html#variant.Left)

`Left`
[if](https://docs.rs/either/1/either/enum.Either.html)

`Either<Self, Self>``into_left(&self)` returns `true`.
Converts `self` into a [variant of](https://docs.rs/either/1/either/enum.Either.html#variant.Right)

`Right`
[otherwise.](https://docs.rs/either/1/either/enum.Either.html)

`Either<Self, Self>`
[Read more](https://docs.rs/either/1/either/into_either/trait.IntoEither.html#method.into_either_with)

## [Source](../../src/polars_utils/parma/raw/key.rs.html#91)
### impl<T> [Key](../../polars_utils/parma/raw/key/trait.Key.html) for Twhere
    T: [Clone](https://doc.rust-lang.org/nightly/core/clone/trait.Clone.html),

[Source](../../src/polars_utils/parma/raw/key.rs.html#91)

### impl<T> [Key](../../polars_utils/parma/raw/key/trait.Key.html) for Twhere
    T: [Clone](https://doc.rust-lang.org/nightly/core/clone/trait.Clone.html),

[Clone](https://doc.rust-lang.org/nightly/core/clone/trait.Clone.html),

##
### impl<T> Pointable for T

### impl<T> Pointable for T

##
### impl<T> PolicyExt for Twhere
    T: ?[Sized](https://doc.rust-lang.org/nightly/core/marker/trait.Sized.html),

### impl<T> PolicyExt for Twhere
    T: ?[Sized](https://doc.rust-lang.org/nightly/core/marker/trait.Sized.html),

[Sized](https://doc.rust-lang.org/nightly/core/marker/trait.Sized.html),

##
### impl<T> WithSubscriber for T

### impl<T> WithSubscriber for T

##
#### fn [with_subscriber]<S>(self, subscriber: S) -> WithDispatch<Self>where
    S: [Into](https://doc.rust-lang.org/nightly/core/convert/trait.Into.html)<Dispatch>,

#### fn [with_subscriber]<S>(self, subscriber: S) -> WithDispatch<Self>where
    S: [Into](https://doc.rust-lang.org/nightly/core/convert/trait.Into.html)<Dispatch>,

[Into](https://doc.rust-lang.org/nightly/core/convert/trait.Into.html)<Dispatch>,

##
#### fn [with_current_subscriber](self) -> WithDispatch<Self>

#### fn [with_current_subscriber](self) -> WithDispatch<Self>

### impl<ST, DT> CastableFrom<ST, Initialized, Initialized> for DT

### impl<ST, DT> CastableFrom<ST, Uninit, Uninit> for DT

### impl<T> Read<Exclusive, BecauseExclusive> for Twhere
    T: ?[Sized](https://doc.rust-lang.org/nightly/core/marker/trait.Sized.html),

[Sized](https://doc.rust-lang.org/nightly/core/marker/trait.Sized.html),

### impl<T> Ungil for Twhere
    T: [Send](https://doc.rust-lang.org/nightly/core/marker/trait.Send.html),

[Send](https://doc.rust-lang.org/nightly/core/marker/trait.Send.html),

# Citations

1. Source page: https://docs.pola.rs/api/rust/dev/polars_lazy/frame/struct.LazyFrame.html
