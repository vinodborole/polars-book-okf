---
type: Web Page
title: DataFrame in polars::frame - Rust
description: A contiguous growable collection of `Column`s that have the same length.
resource: https://docs.pola.rs/api/rust/dev/polars/frame/struct.DataFrame.html
timestamp: '2026-08-24T07:00:57.822031+00:00'
---

`pub struct DataFrame { /* private fields */ }`
## Expand description

A contiguous growable collection of [`Column`](../prelude/enum.Column.html)s that have the same length.

###Use declarations

All the common tools can be found in [`crate::prelude`](../../polars_core/prelude/index.html) (or in `polars::prelude`).

```
use polars_core::prelude::*; // if the crate polars-core is used directly
// use polars::prelude::*;      if the crate polars is used
```
##Initialization

###Default

A `DataFrame` can be initialized empty:

```
let df = DataFrame::empty();
assert_eq!(df.shape(), (0, 0));
```
###Constructing from a `Vec<Column>`

A `DataFrame` is backed by a `Vec<Column>` where the `Column`s have the same length.

```
let s1 = Column::new("Fruit".into(), ["Apple", "Apple", "Pear"]);
let s2 = Column::new("Color".into(), ["Red", "Yellow", "Green"]);
let df: PolarsResult<DataFrame> = DataFrame::new_infer_height(vec![s1, s2]);
```
###Using a macro

The [`df!`](../macro.df.html) macro is a convenient method:

```
let df: PolarsResult<DataFrame> = df!("Fruit" => ["Apple", "Apple", "Pear"],
                                      "Color" => ["Red", "Yellow", "Green"]);
```
###Using a CSV file

See the `polars_io::csv::CsvReader`.

##Indexing

###By a number

The `Index<usize>` is implemented for the `DataFrame`.

```
let df = df!("Fruit" => ["Apple", "Apple", "Pear"],
             "Color" => ["Red", "Yellow", "Green"])?;
assert_eq!(df[0], Column::new("Fruit".into(), &["Apple", "Apple", "Pear"]));
assert_eq!(df[1], Column::new("Color".into(), &["Red", "Yellow", "Green"]));
```
###By a `Series` name

```
let df = df!("Fruit" => ["Apple", "Apple", "Pear"],
             "Color" => ["Red", "Yellow", "Green"])?;
assert_eq!(df["Fruit"], Column::new("Fruit".into(), &["Apple", "Apple", "Pear"]));
assert_eq!(df["Color"], Column::new("Color".into(), &["Red", "Yellow", "Green"]));
```
## Implementations

## [Source](../../src/polars_core/chunked_array/ndarray.rs.html#78)
### impl [DataFrame](../prelude/struct.DataFrame.html)

[Source](../../src/polars_core/chunked_array/ndarray.rs.html#78)

### impl [DataFrame](../prelude/struct.DataFrame.html)

## [Source](../../src/polars_core/chunked_array/ndarray.rs.html#100-102)
#### pub fn [to_ndarray](#method.to_ndarray)<N>(
    &self,
    ordering: [IndexOrder](../prelude/enum.IndexOrder.html),
) -> [Result](https://doc.rust-lang.org/nightly/core/result/enum.Result.html)<[ArrayBase](https://docs.rs/ndarray/0.15/ndarray/struct.ArrayBase.html)<[OwnedRepr](https://docs.rs/ndarray/0.15/ndarray/data_repr/struct.OwnedRepr.html)<<N as [PolarsNumericType](../prelude/trait.PolarsNumericType.html)>::[Native](../prelude/trait.PolarsNumericType.html#associatedtype.Native)>, [Dim](https://docs.rs/ndarray/0.15/ndarray/dimension/dim/struct.Dim.html)<[[usize](https://doc.rust-lang.org/nightly/std/primitive.usize.html); [2](https://doc.rust-lang.org/nightly/std/primitive.array.html)]>>, [PolarsError](../prelude/enum.PolarsError.html)>where
    N: [PolarsNumericType](../prelude/trait.PolarsNumericType.html),

[Source](../../src/polars_core/chunked_array/ndarray.rs.html#100-102)

#### pub fn [to_ndarray](#method.to_ndarray)<N>(
    &self,
    ordering: [IndexOrder](../prelude/enum.IndexOrder.html),
) -> [Result](https://doc.rust-lang.org/nightly/core/result/enum.Result.html)<[ArrayBase](https://docs.rs/ndarray/0.15/ndarray/struct.ArrayBase.html)<[OwnedRepr](https://docs.rs/ndarray/0.15/ndarray/data_repr/struct.OwnedRepr.html)<<N as [PolarsNumericType](../prelude/trait.PolarsNumericType.html)>::[Native](../prelude/trait.PolarsNumericType.html#associatedtype.Native)>, [Dim](https://docs.rs/ndarray/0.15/ndarray/dimension/dim/struct.Dim.html)<[[usize](https://doc.rust-lang.org/nightly/std/primitive.usize.html); [2](https://doc.rust-lang.org/nightly/std/primitive.array.html)]>>, [PolarsError](../prelude/enum.PolarsError.html)>where
    N: [PolarsNumericType](../prelude/trait.PolarsNumericType.html),

[PolarsNumericType](../prelude/trait.PolarsNumericType.html),

Create a 2D [`ndarray::Array`](https://docs.rs/ndarray/0.15/ndarray/type.Array.html) from this [`DataFrame`](../prelude/struct.DataFrame.html). This requires all columns in the
[`DataFrame`](../prelude/struct.DataFrame.html) to be non-null and numeric. They will be cast to the same data type
(if they aren’t already).

For floating point data we implicitly convert `None` to `NaN` without failure.

```
use polars_core::prelude::*;
let a = UInt32Chunked::new("a".into(), &[1, 2, 3]).into_column();
let b = Float64Chunked::new("b".into(), &[10., 8., 6.]).into_column();
let df = DataFrame::new_infer_height(vec![a, b]).unwrap();
let ndarray = df.to_ndarray::<Float64Type>(IndexOrder::Fortran).unwrap();
println!("{:?}", ndarray);
```
Outputs:

```
[[1.0, 10.0],
 [2.0, 8.0],
 [3.0, 6.0]], shape=[3, 2], strides=[1, 3], layout=Ff (0xa), const ndim=2
```
## [Source](../../src/polars_core/chunked_array/random.rs.html#184)
### impl [DataFrame](../prelude/struct.DataFrame.html)

[Source](../../src/polars_core/chunked_array/random.rs.html#184)

### impl [DataFrame](../prelude/struct.DataFrame.html)

## [Source](../../src/polars_core/chunked_array/random.rs.html#186-192)
#### pub fn [sample_n](#method.sample_n)(
    &self,
    n: &[Series](../prelude/struct.Series.html),
    with_replacement: [bool](https://doc.rust-lang.org/nightly/std/primitive.bool.html),
    shuffle: [Option](https://doc.rust-lang.org/nightly/core/option/enum.Option.html)<[bool](https://doc.rust-lang.org/nightly/std/primitive.bool.html)>,
    seed: [Option](https://doc.rust-lang.org/nightly/core/option/enum.Option.html)<[u64](https://doc.rust-lang.org/nightly/std/primitive.u64.html)>,
) -> [Result](https://doc.rust-lang.org/nightly/core/result/enum.Result.html)<[DataFrame](../prelude/struct.DataFrame.html), [PolarsError](../prelude/enum.PolarsError.html)>

[Source](../../src/polars_core/chunked_array/random.rs.html#186-192)

#### pub fn [sample_n](#method.sample_n)(
    &self,
    n: &[Series](../prelude/struct.Series.html),
    with_replacement: [bool](https://doc.rust-lang.org/nightly/std/primitive.bool.html),
    shuffle: [Option](https://doc.rust-lang.org/nightly/core/option/enum.Option.html)<[bool](https://doc.rust-lang.org/nightly/std/primitive.bool.html)>,
    seed: [Option](https://doc.rust-lang.org/nightly/core/option/enum.Option.html)<[u64](https://doc.rust-lang.org/nightly/std/primitive.u64.html)>,
) -> [Result](https://doc.rust-lang.org/nightly/core/result/enum.Result.html)<[DataFrame](../prelude/struct.DataFrame.html), [PolarsError](../prelude/enum.PolarsError.html)>

Sample n datapoints from this [`DataFrame`](../prelude/struct.DataFrame.html).

[Source](../../src/polars_core/chunked_array/random.rs.html#207-213)

#### pub fn [sample_n_literal](#method.sample_n_literal)(
    &self,
    n: [usize](https://doc.rust-lang.org/nightly/std/primitive.usize.html),
    with_replacement: [bool](https://doc.rust-lang.org/nightly/std/primitive.bool.html),
    shuffle: [Option](https://doc.rust-lang.org/nightly/core/option/enum.Option.html)<[bool](https://doc.rust-lang.org/nightly/std/primitive.bool.html)>,
    seed: [Option](https://doc.rust-lang.org/nightly/core/option/enum.Option.html)<[u64](https://doc.rust-lang.org/nightly/std/primitive.u64.html)>,
) -> [Result](https://doc.rust-lang.org/nightly/core/result/enum.Result.html)<[DataFrame](../prelude/struct.DataFrame.html), [PolarsError](../prelude/enum.PolarsError.html)>

## [Source](../../src/polars_core/chunked_array/struct_/frame.rs.html#6)
### impl [DataFrame](../prelude/struct.DataFrame.html)

[Source](../../src/polars_core/chunked_array/struct_/frame.rs.html#6)

### impl [DataFrame](../prelude/struct.DataFrame.html)

[Source](../../src/polars_core/chunked_array/struct_/frame.rs.html#7)

#### pub fn [into_struct](#method.into_struct)(self, name: [PlSmallStr](../prelude/struct.PlSmallStr.html)) -> [ChunkedArray](../prelude/struct.ChunkedArray.html)<[StructType](../prelude/struct.StructType.html)>

## [Source](../../src/polars_core/frame/chunks.rs.html#37)
### impl [DataFrame](../prelude/struct.DataFrame.html)

[Source](../../src/polars_core/frame/chunks.rs.html#37)

### impl [DataFrame](../prelude/struct.DataFrame.html)

[Source](../../src/polars_core/frame/chunks.rs.html#38)

#### pub fn [split_chunks](#method.split_chunks)(&mut self) -> impl [Iterator](https://doc.rust-lang.org/nightly/core/iter/traits/iterator/trait.Iterator.html)<Item = [DataFrame](../prelude/struct.DataFrame.html)>

[Source](../../src/polars_core/frame/chunks.rs.html#68)

#### pub fn [split_chunks_by_n](#method.split_chunks_by_n)(self, n: [usize](https://doc.rust-lang.org/nightly/std/primitive.usize.html), parallel: [bool](https://doc.rust-lang.org/nightly/std/primitive.bool.html)) -> [Vec](https://doc.rust-lang.org/nightly/alloc/vec/struct.Vec.html)<[DataFrame](../prelude/struct.DataFrame.html)>

## [Source](../../src/polars_core/frame/chunks.rs.html#82)
#### pub fn [rechunk_to_arrow](#method.rechunk_to_arrow)(&self, compat_level: [CompatLevel](../prelude/struct.CompatLevel.html)) -> [Vec](https://doc.rust-lang.org/nightly/alloc/vec/struct.Vec.html)<[Box](https://doc.rust-lang.org/nightly/alloc/boxed/struct.Box.html)<dyn Array>>

[Source](../../src/polars_core/frame/chunks.rs.html#82)

#### pub fn [rechunk_to_arrow](#method.rechunk_to_arrow)(&self, compat_level: [CompatLevel](../prelude/struct.CompatLevel.html)) -> [Vec](https://doc.rust-lang.org/nightly/alloc/vec/struct.Vec.html)<[Box](https://doc.rust-lang.org/nightly/alloc/boxed/struct.Box.html)<dyn Array>>

Convert the columns of this [DataFrame](../prelude/struct.DataFrame.html) to arrow arrays.

## [Source](../../src/polars_core/frame/chunks.rs.html#90)
#### pub fn [rechunk_into_arrow](#method.rechunk_into_arrow)(
    self,
    compat_level: [CompatLevel](../prelude/struct.CompatLevel.html),
) -> [Vec](https://doc.rust-lang.org/nightly/alloc/vec/struct.Vec.html)<[Box](https://doc.rust-lang.org/nightly/alloc/boxed/struct.Box.html)<dyn Array>>

[Source](../../src/polars_core/frame/chunks.rs.html#90)

#### pub fn [rechunk_into_arrow](#method.rechunk_into_arrow)(
    self,
    compat_level: [CompatLevel](../prelude/struct.CompatLevel.html),
) -> [Vec](https://doc.rust-lang.org/nightly/alloc/vec/struct.Vec.html)<[Box](https://doc.rust-lang.org/nightly/alloc/boxed/struct.Box.html)<dyn Array>>

Convert the columns of this [DataFrame](../prelude/struct.DataFrame.html) to arrow arrays.

## [Source](../../src/polars_core/frame/dataframe.rs.html#99)
### impl [DataFrame](../prelude/struct.DataFrame.html)

[Source](../../src/polars_core/frame/dataframe.rs.html#99)

### impl [DataFrame](../prelude/struct.DataFrame.html)

## [Source](../../src/polars_core/frame/dataframe.rs.html#108)
#### pub const fn [empty](#method.empty)() -> [DataFrame](../prelude/struct.DataFrame.html)

[Source](../../src/polars_core/frame/dataframe.rs.html#108)

#### pub const fn [empty](#method.empty)() -> [DataFrame](../prelude/struct.DataFrame.html)

Creates an empty `DataFrame` usable in a compile time context (such as static initializers).

#####Example

```
use polars_core::prelude::DataFrame;
static EMPTY: DataFrame = DataFrame::empty();
```
[Source](../../src/polars_core/frame/dataframe.rs.html#112)

#### pub const fn [empty_with_height](#method.empty_with_height)(height: [usize](https://doc.rust-lang.org/nightly/std/primitive.usize.html)) -> [DataFrame](../prelude/struct.DataFrame.html)

[Source](../../src/polars_core/frame/dataframe.rs.html#120)

#### pub fn [new](#method.new)(
    height: [usize](https://doc.rust-lang.org/nightly/std/primitive.usize.html),
    columns: [Vec](https://doc.rust-lang.org/nightly/alloc/vec/struct.Vec.html)<[Column](../prelude/enum.Column.html)>,
) -> [Result](https://doc.rust-lang.org/nightly/core/result/enum.Result.html)<[DataFrame](../prelude/struct.DataFrame.html), [PolarsError](../prelude/enum.PolarsError.html)>

## [Source](../../src/polars_core/frame/dataframe.rs.html#128)
#### pub fn [new_infer_height](#method.new_infer_height)(columns: [Vec](https://doc.rust-lang.org/nightly/alloc/vec/struct.Vec.html)<[Column](../prelude/enum.Column.html)>) -> [Result](https://doc.rust-lang.org/nightly/core/result/enum.Result.html)<[DataFrame](../prelude/struct.DataFrame.html), [PolarsError](../prelude/enum.PolarsError.html)>

[Source](../../src/polars_core/frame/dataframe.rs.html#128)

#### pub fn [new_infer_height](#method.new_infer_height)(columns: [Vec](https://doc.rust-lang.org/nightly/alloc/vec/struct.Vec.html)<[Column](../prelude/enum.Column.html)>) -> [Result](https://doc.rust-lang.org/nightly/core/result/enum.Result.html)<[DataFrame](../prelude/struct.DataFrame.html), [PolarsError](../prelude/enum.PolarsError.html)>

Height is sourced from first column.

## [Source](../../src/polars_core/frame/dataframe.rs.html#149)
#### pub unsafe fn [new_unchecked_infer_height](#method.new_unchecked_infer_height)(columns: [Vec](https://doc.rust-lang.org/nightly/alloc/vec/struct.Vec.html)<[Column](../prelude/enum.Column.html)>) -> [DataFrame](../prelude/struct.DataFrame.html)

[Source](../../src/polars_core/frame/dataframe.rs.html#149)

#### pub unsafe fn [new_unchecked_infer_height](#method.new_unchecked_infer_height)(columns: [Vec](https://doc.rust-lang.org/nightly/alloc/vec/struct.Vec.html)<[Column](../prelude/enum.Column.html)>) -> [DataFrame](../prelude/struct.DataFrame.html)

## [Source](../../src/polars_core/frame/dataframe.rs.html#157)
#### pub const unsafe fn [_new_unchecked_impl](#method._new_unchecked_impl)(
    height: [usize](https://doc.rust-lang.org/nightly/std/primitive.usize.html),
    columns: [Vec](https://doc.rust-lang.org/nightly/alloc/vec/struct.Vec.html)<[Column](../prelude/enum.Column.html)>,
) -> [DataFrame](../prelude/struct.DataFrame.html)

[Source](../../src/polars_core/frame/dataframe.rs.html#157)

#### pub const unsafe fn [_new_unchecked_impl](#method._new_unchecked_impl)(
    height: [usize](https://doc.rust-lang.org/nightly/std/primitive.usize.html),
    columns: [Vec](https://doc.rust-lang.org/nightly/alloc/vec/struct.Vec.html)<[Column](../prelude/enum.Column.html)>,
) -> [DataFrame](../prelude/struct.DataFrame.html)

This will not panic even in debug mode - there are some (rare) use cases where a DataFrame is temporarily constructed containing duplicates for dispatching to functions. A DataFrame constructed with this method is generally highly unsafe and should not be long-lived.

## [Source](../../src/polars_core/frame/dataframe.rs.html#167)
#### pub fn [new_with_broadcast](#method.new_with_broadcast)(
    height: [usize](https://doc.rust-lang.org/nightly/std/primitive.usize.html),
    columns: [Vec](https://doc.rust-lang.org/nightly/alloc/vec/struct.Vec.html)<[Column](../prelude/enum.Column.html)>,
) -> [Result](https://doc.rust-lang.org/nightly/core/result/enum.Result.html)<[DataFrame](../prelude/struct.DataFrame.html), [PolarsError](../prelude/enum.PolarsError.html)>

[Source](../../src/polars_core/frame/dataframe.rs.html#167)

#### pub fn [new_with_broadcast](#method.new_with_broadcast)(
    height: [usize](https://doc.rust-lang.org/nightly/std/primitive.usize.html),
    columns: [Vec](https://doc.rust-lang.org/nightly/alloc/vec/struct.Vec.html)<[Column](../prelude/enum.Column.html)>,
) -> [Result](https://doc.rust-lang.org/nightly/core/result/enum.Result.html)<[DataFrame](../prelude/struct.DataFrame.html), [PolarsError](../prelude/enum.PolarsError.html)>

Broadcasts unit-length columns to `height`. Errors if a column has height that is non-unit
length and not equal to `self.height()`.

## [Source](../../src/polars_core/frame/dataframe.rs.html#175)
#### pub fn [new_infer_broadcast](#method.new_infer_broadcast)(
    columns: [Vec](https://doc.rust-lang.org/nightly/alloc/vec/struct.Vec.html)<[Column](../prelude/enum.Column.html)>,
) -> [Result](https://doc.rust-lang.org/nightly/core/result/enum.Result.html)<[DataFrame](../prelude/struct.DataFrame.html), [PolarsError](../prelude/enum.PolarsError.html)>

[Source](../../src/polars_core/frame/dataframe.rs.html#175)

#### pub fn [new_infer_broadcast](#method.new_infer_broadcast)(
    columns: [Vec](https://doc.rust-lang.org/nightly/alloc/vec/struct.Vec.html)<[Column](../prelude/enum.Column.html)>,
) -> [Result](https://doc.rust-lang.org/nightly/core/result/enum.Result.html)<[DataFrame](../prelude/struct.DataFrame.html), [PolarsError](../prelude/enum.PolarsError.html)>

Infers height as the first non-unit length column or 1 if not found.

## [Source](../../src/polars_core/frame/dataframe.rs.html#185-188)
#### pub unsafe fn [new_unchecked_with_broadcast](#method.new_unchecked_with_broadcast)(
    height: [usize](https://doc.rust-lang.org/nightly/std/primitive.usize.html),
    columns: [Vec](https://doc.rust-lang.org/nightly/alloc/vec/struct.Vec.html)<[Column](../prelude/enum.Column.html)>,
) -> [Result](https://doc.rust-lang.org/nightly/core/result/enum.Result.html)<[DataFrame](../prelude/struct.DataFrame.html), [PolarsError](../prelude/enum.PolarsError.html)>

[Source](../../src/polars_core/frame/dataframe.rs.html#185-188)

#### pub unsafe fn [new_unchecked_with_broadcast](#method.new_unchecked_with_broadcast)(
    height: [usize](https://doc.rust-lang.org/nightly/std/primitive.usize.html),
    columns: [Vec](https://doc.rust-lang.org/nightly/alloc/vec/struct.Vec.html)<[Column](../prelude/enum.Column.html)>,
) -> [Result](https://doc.rust-lang.org/nightly/core/result/enum.Result.html)<[DataFrame](../prelude/struct.DataFrame.html), [PolarsError](../prelude/enum.PolarsError.html)>

## [Source](../../src/polars_core/frame/dataframe.rs.html#197)
#### pub unsafe fn [new_unchecked_infer_broadcast](#method.new_unchecked_infer_broadcast)(
    columns: [Vec](https://doc.rust-lang.org/nightly/alloc/vec/struct.Vec.html)<[Column](../prelude/enum.Column.html)>,
) -> [Result](https://doc.rust-lang.org/nightly/core/result/enum.Result.html)<[DataFrame](../prelude/struct.DataFrame.html), [PolarsError](../prelude/enum.PolarsError.html)>

[Source](../../src/polars_core/frame/dataframe.rs.html#197)

#### pub unsafe fn [new_unchecked_infer_broadcast](#method.new_unchecked_infer_broadcast)(
    columns: [Vec](https://doc.rust-lang.org/nightly/alloc/vec/struct.Vec.html)<[Column](../prelude/enum.Column.html)>,
) -> [Result](https://doc.rust-lang.org/nightly/core/result/enum.Result.html)<[DataFrame](../prelude/struct.DataFrame.html), [PolarsError](../prelude/enum.PolarsError.html)>

## [Source](../../src/polars_core/frame/dataframe.rs.html#205)
#### pub fn [broadcast_to](#method.broadcast_to)(
    &self,
    height: [usize](https://doc.rust-lang.org/nightly/std/primitive.usize.html),
) -> [Result](https://doc.rust-lang.org/nightly/core/result/enum.Result.html)<[Cow](https://doc.rust-lang.org/nightly/alloc/borrow/enum.Cow.html)<'_, [DataFrame](../prelude/struct.DataFrame.html)>, [PolarsError](../prelude/enum.PolarsError.html)>

[Source](../../src/polars_core/frame/dataframe.rs.html#205)

#### pub fn [broadcast_to](#method.broadcast_to)(
    &self,
    height: [usize](https://doc.rust-lang.org/nightly/std/primitive.usize.html),
) -> [Result](https://doc.rust-lang.org/nightly/core/result/enum.Result.html)<[Cow](https://doc.rust-lang.org/nightly/alloc/borrow/enum.Cow.html)<'_, [DataFrame](../prelude/struct.DataFrame.html)>, [PolarsError](../prelude/enum.PolarsError.html)>

Returns a DataFrame with the given height.

Errors if this DataFrame’s height is not 1 and also not equal to the requested height.

## [Source](../../src/polars_core/frame/dataframe.rs.html#219)
#### pub fn [broadcast_in_place_to](#method.broadcast_in_place_to)(
    &mut self,
    length: [usize](https://doc.rust-lang.org/nightly/std/primitive.usize.html),
) -> [Result](https://doc.rust-lang.org/nightly/core/result/enum.Result.html)<[()](https://doc.rust-lang.org/nightly/std/primitive.unit.html), [PolarsError](../prelude/enum.PolarsError.html)>

[Source](../../src/polars_core/frame/dataframe.rs.html#219)

#### pub fn [broadcast_in_place_to](#method.broadcast_in_place_to)(
    &mut self,
    length: [usize](https://doc.rust-lang.org/nightly/std/primitive.usize.html),
) -> [Result](https://doc.rust-lang.org/nightly/core/result/enum.Result.html)<[()](https://doc.rust-lang.org/nightly/std/primitive.unit.html), [PolarsError](../prelude/enum.PolarsError.html)>

See broadcast_to.

## [Source](../../src/polars_core/frame/dataframe.rs.html#227)
#### pub fn [broadcast_owned_to](#method.broadcast_owned_to)(self, length: [usize](https://doc.rust-lang.org/nightly/std/primitive.usize.html)) -> [Result](https://doc.rust-lang.org/nightly/core/result/enum.Result.html)<[DataFrame](../prelude/struct.DataFrame.html), [PolarsError](../prelude/enum.PolarsError.html)>

[Source](../../src/polars_core/frame/dataframe.rs.html#227)

#### pub fn [broadcast_owned_to](#method.broadcast_owned_to)(self, length: [usize](https://doc.rust-lang.org/nightly/std/primitive.usize.html)) -> [Result](https://doc.rust-lang.org/nightly/core/result/enum.Result.html)<[DataFrame](../prelude/struct.DataFrame.html), [PolarsError](../prelude/enum.PolarsError.html)>

See broadcast_to.

## [Source](../../src/polars_core/frame/dataframe.rs.html#233)
#### pub fn [empty_with_schema](#method.empty_with_schema)(schema: &Schema<[DataType](../prelude/enum.DataType.html), [()](https://doc.rust-lang.org/nightly/std/primitive.unit.html)>) -> [DataFrame](../prelude/struct.DataFrame.html)

[Source](../../src/polars_core/frame/dataframe.rs.html#233)

#### pub fn [empty_with_schema](#method.empty_with_schema)(schema: &Schema<[DataType](../prelude/enum.DataType.html), [()](https://doc.rust-lang.org/nightly/std/primitive.unit.html)>) -> [DataFrame](../prelude/struct.DataFrame.html)

Create a `DataFrame` 0 height and columns as per the `schema`.

## [Source](../../src/polars_core/frame/dataframe.rs.html#243)
#### pub fn [empty_with_arc_schema](#method.empty_with_arc_schema)(schema: [Arc](../prelude/struct.Arc.html)<Schema<[DataType](../prelude/enum.DataType.html), [()](https://doc.rust-lang.org/nightly/std/primitive.unit.html)>>) -> [DataFrame](../prelude/struct.DataFrame.html)

[Source](../../src/polars_core/frame/dataframe.rs.html#243)

#### pub fn [empty_with_arc_schema](#method.empty_with_arc_schema)(schema: [Arc](../prelude/struct.Arc.html)<Schema<[DataType](../prelude/enum.DataType.html), [()](https://doc.rust-lang.org/nightly/std/primitive.unit.html)>>) -> [DataFrame](../prelude/struct.DataFrame.html)

Create an empty `DataFrame` with empty columns as per the `schema`.

## [Source](../../src/polars_core/frame/dataframe.rs.html#255)
#### pub unsafe fn [set_height](#method.set_height)(&mut self, height: [usize](https://doc.rust-lang.org/nightly/std/primitive.usize.html)) -> &mut [DataFrame](../prelude/struct.DataFrame.html)

[Source](../../src/polars_core/frame/dataframe.rs.html#255)

#### pub unsafe fn [set_height](#method.set_height)(&mut self, height: [usize](https://doc.rust-lang.org/nightly/std/primitive.usize.html)) -> &mut [DataFrame](../prelude/struct.DataFrame.html)

## [Source](../../src/polars_core/frame/dataframe.rs.html#289)
#### pub fn [shape](#method.shape)(&self) -> ([usize](https://doc.rust-lang.org/nightly/std/primitive.usize.html), [usize](https://doc.rust-lang.org/nightly/std/primitive.usize.html))

[Source](../../src/polars_core/frame/dataframe.rs.html#289)

#### pub fn [shape](#method.shape)(&self) -> ([usize](https://doc.rust-lang.org/nightly/std/primitive.usize.html), [usize](https://doc.rust-lang.org/nightly/std/primitive.usize.html))

Get (height, width) of the [`DataFrame`](../prelude/struct.DataFrame.html).

#####Example

```
let df0: DataFrame = DataFrame::empty();
let df1: DataFrame = df!("1" => [1, 2, 3, 4, 5])?;
let df2: DataFrame = df!("1" => [1, 2, 3, 4, 5],
                         "2" => [1, 2, 3, 4, 5])?;
assert_eq!(df0.shape(), (0 ,0));
assert_eq!(df1.shape(), (5, 1));
assert_eq!(df2.shape(), (5, 2));
```
## [Source](../../src/polars_core/frame/dataframe.rs.html#295)
#### pub fn [shape_has_zero](#method.shape_has_zero)(&self) -> [bool](https://doc.rust-lang.org/nightly/std/primitive.bool.html)

[Source](../../src/polars_core/frame/dataframe.rs.html#295)

#### pub fn [shape_has_zero](#method.shape_has_zero)(&self) -> [bool](https://doc.rust-lang.org/nightly/std/primitive.bool.html)

0 width or height.

[Source](../../src/polars_core/frame/dataframe.rs.html#300)

#### pub fn [columns](#method.columns)(&self) -> &[[Column](../prelude/enum.Column.html)]

[Source](../../src/polars_core/frame/dataframe.rs.html#305)

#### pub fn [into_columns](#method.into_columns)(self) -> [Vec](https://doc.rust-lang.org/nightly/alloc/vec/struct.Vec.html)<[Column](../prelude/enum.Column.html)>

## [Source](../../src/polars_core/frame/dataframe.rs.html#314)
#### pub unsafe fn [columns_mut](#method.columns_mut)(&mut self) -> &mut [Vec](https://doc.rust-lang.org/nightly/alloc/vec/struct.Vec.html)<[Column](../prelude/enum.Column.html)>

[Source](../../src/polars_core/frame/dataframe.rs.html#314)

#### pub unsafe fn [columns_mut](#method.columns_mut)(&mut self) -> &mut [Vec](https://doc.rust-lang.org/nightly/alloc/vec/struct.Vec.html)<[Column](../prelude/enum.Column.html)>

#####Safety

The caller must ensure the length of all [`Column`](../prelude/enum.Column.html)s remains equal to `self.height`, or
that [`DataFrame::set_height`](../prelude/struct.DataFrame.html#method.set_height) is called afterwards with the new `height`.

## [Source](../../src/polars_core/frame/dataframe.rs.html#323)
#### pub unsafe fn [columns_mut_retain_schema](#method.columns_mut_retain_schema)(&mut self) -> &mut [Vec](https://doc.rust-lang.org/nightly/alloc/vec/struct.Vec.html)<[Column](../prelude/enum.Column.html)>

[Source](../../src/polars_core/frame/dataframe.rs.html#323)

#### pub unsafe fn [columns_mut_retain_schema](#method.columns_mut_retain_schema)(&mut self) -> &mut [Vec](https://doc.rust-lang.org/nightly/alloc/vec/struct.Vec.html)<[Column](../prelude/enum.Column.html)>

#####Safety

Adheres to all safety requirements of [`DataFrame::columns_mut`](../prelude/struct.DataFrame.html#method.columns_mut), and that the list of column
names remains unchanged.

[Source](../../src/polars_core/frame/dataframe.rs.html#349)

#### pub fn [cached_schema](#method.cached_schema)(&self) -> [Option](https://doc.rust-lang.org/nightly/core/option/enum.Option.html)<&[Arc](../prelude/struct.Arc.html)<Schema<[DataType](../prelude/enum.DataType.html), [()](https://doc.rust-lang.org/nightly/std/primitive.unit.html)>>>

## [Source](../../src/polars_core/frame/dataframe.rs.html#378)
#### pub unsafe fn [set_opt_schema](#method.set_opt_schema)(
    &mut self,
    schema: [Option](https://doc.rust-lang.org/nightly/core/option/enum.Option.html)<[Arc](../prelude/struct.Arc.html)<Schema<[DataType](../prelude/enum.DataType.html), [()](https://doc.rust-lang.org/nightly/std/primitive.unit.html)>>>,
) -> &mut [DataFrame](../prelude/struct.DataFrame.html)

[Source](../../src/polars_core/frame/dataframe.rs.html#378)

#### pub unsafe fn [set_opt_schema](#method.set_opt_schema)(
    &mut self,
    schema: [Option](https://doc.rust-lang.org/nightly/core/option/enum.Option.html)<[Arc](../prelude/struct.Arc.html)<Schema<[DataType](../prelude/enum.DataType.html), [()](https://doc.rust-lang.org/nightly/std/primitive.unit.html)>>>,
) -> &mut [DataFrame](../prelude/struct.DataFrame.html)

## [Source](../../src/polars_core/frame/dataframe.rs.html#391)
#### pub unsafe fn [set_schema_from](#method.set_schema_from)(&mut self, from: &[DataFrame](../prelude/struct.DataFrame.html)) -> &mut [DataFrame](../prelude/struct.DataFrame.html)

[Source](../../src/polars_core/frame/dataframe.rs.html#391)

#### pub unsafe fn [set_schema_from](#method.set_schema_from)(&mut self, from: &[DataFrame](../prelude/struct.DataFrame.html)) -> &mut [DataFrame](../prelude/struct.DataFrame.html)

Clones the cached schema from `from` to `self.cached_schema` if there is one.

#####Safety

Schema must match the columns in `self`.

## [Source](../../src/polars_core/frame/dataframe.rs.html#401)
#### pub unsafe fn [with_schema_from](#method.with_schema_from)(self, from: &[DataFrame](../prelude/struct.DataFrame.html)) -> [DataFrame](../prelude/struct.DataFrame.html)

[Source](../../src/polars_core/frame/dataframe.rs.html#401)

#### pub unsafe fn [with_schema_from](#method.with_schema_from)(self, from: &[DataFrame](../prelude/struct.DataFrame.html)) -> [DataFrame](../prelude/struct.DataFrame.html)

Clones the cached schema from `from` to `self.cached_schema` if there is one.

#####Safety

Schema must match the columns in `self`.

## [Source](../../src/polars_core/frame/explode.rs.html#60)
### impl [DataFrame](../prelude/struct.DataFrame.html)

[Source](../../src/polars_core/frame/explode.rs.html#60)

### impl [DataFrame](../prelude/struct.DataFrame.html)

[Source](../../src/polars_core/frame/explode.rs.html#61-65)

#### pub fn [explode_impl](#method.explode_impl)(
    &self,
    columns: [Vec](https://doc.rust-lang.org/nightly/alloc/vec/struct.Vec.html)<[Column](../prelude/enum.Column.html)>,
    options: [ExplodeOptions](../prelude/struct.ExplodeOptions.html),
) -> [Result](https://doc.rust-lang.org/nightly/core/result/enum.Result.html)<[DataFrame](../prelude/struct.DataFrame.html), [PolarsError](../prelude/enum.PolarsError.html)>

## [Source](../../src/polars_core/frame/explode.rs.html#219-222)
#### pub fn [explode](#method.explode)<I, S>(
    &self,
    columns: I,
    options: [ExplodeOptions](../prelude/struct.ExplodeOptions.html),
) -> [Result](https://doc.rust-lang.org/nightly/core/result/enum.Result.html)<[DataFrame](../prelude/struct.DataFrame.html), [PolarsError](../prelude/enum.PolarsError.html)>

[Source](../../src/polars_core/frame/explode.rs.html#219-222)

#### pub fn [explode](#method.explode)<I, S>(
    &self,
    columns: I,
    options: [ExplodeOptions](../prelude/struct.ExplodeOptions.html),
) -> [Result](https://doc.rust-lang.org/nightly/core/result/enum.Result.html)<[DataFrame](../prelude/struct.DataFrame.html), [PolarsError](../prelude/enum.PolarsError.html)>

Explode `DataFrame` to long format by exploding a column with Lists.

#####Example

[ⓘ](#)

```
let s0 = Series::new("a".into(), &[1i64, 2, 3]);
let s1 = Series::new("b".into(), &[1i64, 1, 1]);
let s2 = Series::new("c".into(), &[2i64, 2, 2]);
let list = Series::new("foo", &[s0, s1, s2]);
let s0 = Series::new("B".into(), [1, 2, 3]);
let s1 = Series::new("C".into(), [1, 1, 1]);
let df = DataFrame::new_infer_height(vec![list, s0, s1])?;
let exploded = df.explode(["foo"])?;
println!("{:?}", df);
println!("{:?}", exploded);
```
Outputs:

```
 +-------------+-----+-----+
 | foo         | B   | C   |
 | ---         | --- | --- |
 | list [i64]  | i32 | i32 |
 +=============+=====+=====+
 | "[1, 2, 3]" | 1   | 1   |
 +-------------+-----+-----+
 | "[1, 1, 1]" | 2   | 1   |
 +-------------+-----+-----+
 | "[2, 2, 2]" | 3   | 1   |
 +-------------+-----+-----+
 +-----+-----+-----+
 | foo | B   | C   |
 | --- | --- | --- |
 | i64 | i32 | i32 |
 +=====+=====+=====+
 | 1   | 1   | 1   |
 +-----+-----+-----+
 | 2   | 1   | 1   |
 +-----+-----+-----+
 | 3   | 1   | 1   |
 +-----+-----+-----+
 | 1   | 2   | 1   |
 +-----+-----+-----+
 | 1   | 2   | 1   |
 +-----+-----+-----+
 | 1   | 2   | 1   |
 +-----+-----+-----+
 | 2   | 3   | 1   |
 +-----+-----+-----+
 | 2   | 3   | 1   |
 +-----+-----+-----+
 | 2   | 3   | 1   |
 +-----+-----+-----+
```
## [Source](../../src/polars_core/frame/group_by/mod.rs.html#28)
### impl [DataFrame](../prelude/struct.DataFrame.html)

[Source](../../src/polars_core/frame/group_by/mod.rs.html#28)

### impl [DataFrame](../prelude/struct.DataFrame.html)

[Source](../../src/polars_core/frame/group_by/mod.rs.html#29-34)

#### pub fn [group_by_with_series](#method.group_by_with_series)(
    &self,
    by: [Vec](https://doc.rust-lang.org/nightly/alloc/vec/struct.Vec.html)<[Column](../prelude/enum.Column.html)>,
    multithreaded: [bool](https://doc.rust-lang.org/nightly/std/primitive.bool.html),
    sorted: [bool](https://doc.rust-lang.org/nightly/std/primitive.bool.html),
) -> [Result](https://doc.rust-lang.org/nightly/core/result/enum.Result.html)<[GroupBy](../prelude/struct.GroupBy.html)<'_>, [PolarsError](../prelude/enum.PolarsError.html)>

## [Source](../../src/polars_core/frame/group_by/mod.rs.html#112-115)
#### pub fn [group_by](#method.group_by)<I, S>(&self, by: I) -> [Result](https://doc.rust-lang.org/nightly/core/result/enum.Result.html)<[GroupBy](../prelude/struct.GroupBy.html)<'_>, [PolarsError](../prelude/enum.PolarsError.html)>

[Source](../../src/polars_core/frame/group_by/mod.rs.html#112-115)

#### pub fn [group_by](#method.group_by)<I, S>(&self, by: I) -> [Result](https://doc.rust-lang.org/nightly/core/result/enum.Result.html)<[GroupBy](../prelude/struct.GroupBy.html)<'_>, [PolarsError](../prelude/enum.PolarsError.html)>

Group DataFrame using a Series column.

#####Example

```
use polars_core::prelude::*;
fn group_by_sum(df: &DataFrame) -> PolarsResult<DataFrame> {
    df.group_by(["column_name"])?
    .select(["agg_column_name"])
    .sum()
}
```
## [Source](../../src/polars_core/frame/group_by/mod.rs.html#123-126)
#### pub fn [group_by_stable](#method.group_by_stable)<I, S>(&self, by: I) -> [Result](https://doc.rust-lang.org/nightly/core/result/enum.Result.html)<[GroupBy](../prelude/struct.GroupBy.html)<'_>, [PolarsError](../prelude/enum.PolarsError.html)>

[Source](../../src/polars_core/frame/group_by/mod.rs.html#123-126)

#### pub fn [group_by_stable](#method.group_by_stable)<I, S>(&self, by: I) -> [Result](https://doc.rust-lang.org/nightly/core/result/enum.Result.html)<[GroupBy](../prelude/struct.GroupBy.html)<'_>, [PolarsError](../prelude/enum.PolarsError.html)>

Group DataFrame using a Series column. The groups are ordered by their smallest row index.

## [Source](../../src/polars_core/frame/horizontal.rs.html#8)
### impl [DataFrame](../prelude/struct.DataFrame.html)

[Source](../../src/polars_core/frame/horizontal.rs.html#8)

### impl [DataFrame](../prelude/struct.DataFrame.html)

## [Source](../../src/polars_core/frame/horizontal.rs.html#20)
#### pub unsafe fn [hstack_mut_unchecked](#method.hstack_mut_unchecked)(
    &mut self,
    columns: &[[Column](../prelude/enum.Column.html)],
) -> &mut [DataFrame](../prelude/struct.DataFrame.html)

[Source](../../src/polars_core/frame/horizontal.rs.html#20)

#### pub unsafe fn [hstack_mut_unchecked](#method.hstack_mut_unchecked)(
    &mut self,
    columns: &[[Column](../prelude/enum.Column.html)],
) -> &mut [DataFrame](../prelude/struct.DataFrame.html)

Add columns horizontally.

#####Safety

The caller must ensure:

Note: If `self` is empty, `self.height` will always be overridden by the height of the first
column in `columns`.

Note that on a debug build this will panic on duplicates / height mismatch.

## [Source](../../src/polars_core/frame/horizontal.rs.html#54)
#### pub fn [hstack_mut](#method.hstack_mut)(
    &mut self,
    columns: &[[Column](../prelude/enum.Column.html)],
) -> [Result](https://doc.rust-lang.org/nightly/core/result/enum.Result.html)<&mut [DataFrame](../prelude/struct.DataFrame.html), [PolarsError](../prelude/enum.PolarsError.html)>

[Source](../../src/polars_core/frame/horizontal.rs.html#54)

#### pub fn [hstack_mut](#method.hstack_mut)(
    &mut self,
    columns: &[[Column](../prelude/enum.Column.html)],
) -> [Result](https://doc.rust-lang.org/nightly/core/result/enum.Result.html)<&mut [DataFrame](../prelude/struct.DataFrame.html), [PolarsError](../prelude/enum.PolarsError.html)>

Add multiple [`Column`](../prelude/enum.Column.html) to a [`DataFrame`](../prelude/struct.DataFrame.html).
Errors if the resulting DataFrame columns have duplicate names or unequal heights.

Note: If `self` is empty, `self.height` will always be overridden by the height of the first
column in `columns`.

#####Example

```
fn stack(df: &mut DataFrame, columns: &[Column]) {
    df.hstack_mut(columns);
}
```
## [Source](../../src/polars_core/frame/row/dataframe.rs.html#3)
### impl [DataFrame](../prelude/struct.DataFrame.html)

[Source](../../src/polars_core/frame/row/dataframe.rs.html#3)

### impl [DataFrame](../prelude/struct.DataFrame.html)

## [Source](../../src/polars_core/frame/row/dataframe.rs.html#5)
#### pub fn [get_row](#method.get_row)(&self, idx: [usize](https://doc.rust-lang.org/nightly/std/primitive.usize.html)) -> [Result](https://doc.rust-lang.org/nightly/core/result/enum.Result.html)<[Row](row/struct.Row.html)<'_>, [PolarsError](../prelude/enum.PolarsError.html)>

[Source](../../src/polars_core/frame/row/dataframe.rs.html#5)

#### pub fn [get_row](#method.get_row)(&self, idx: [usize](https://doc.rust-lang.org/nightly/std/primitive.usize.html)) -> [Result](https://doc.rust-lang.org/nightly/core/result/enum.Result.html)<[Row](row/struct.Row.html)<'_>, [PolarsError](../prelude/enum.PolarsError.html)>

Get a row from a [`DataFrame`](../prelude/struct.DataFrame.html). Use of this is discouraged as it will likely be slow.

## [Source](../../src/polars_core/frame/row/dataframe.rs.html#16)
#### pub fn [get_row_amortized](#method.get_row_amortized)<'a>(
    &'a self,
    idx: [usize](https://doc.rust-lang.org/nightly/std/primitive.usize.html),
    row: &mut [Row](row/struct.Row.html)<'a>,
) -> [Result](https://doc.rust-lang.org/nightly/core/result/enum.Result.html)<[()](https://doc.rust-lang.org/nightly/std/primitive.unit.html), [PolarsError](../prelude/enum.PolarsError.html)>

[Source](../../src/polars_core/frame/row/dataframe.rs.html#16)

#### pub fn [get_row_amortized](#method.get_row_amortized)<'a>(
    &'a self,
    idx: [usize](https://doc.rust-lang.org/nightly/std/primitive.usize.html),
    row: &mut [Row](row/struct.Row.html)<'a>,
) -> [Result](https://doc.rust-lang.org/nightly/core/result/enum.Result.html)<[()](https://doc.rust-lang.org/nightly/std/primitive.unit.html), [PolarsError](../prelude/enum.PolarsError.html)>

Amortize allocations by reusing a row.
The caller is responsible to make sure that the row has at least the capacity for the number
of columns in the `DataFrame`

## [Source](../../src/polars_core/frame/row/dataframe.rs.html#30)
#### pub unsafe fn [get_row_amortized_unchecked](#method.get_row_amortized_unchecked)<'a>(
    &'a self,
    idx: [usize](https://doc.rust-lang.org/nightly/std/primitive.usize.html),
    row: &mut [Row](row/struct.Row.html)<'a>,
)

[Source](../../src/polars_core/frame/row/dataframe.rs.html#30)

#### pub unsafe fn [get_row_amortized_unchecked](#method.get_row_amortized_unchecked)<'a>(
    &'a self,
    idx: [usize](https://doc.rust-lang.org/nightly/std/primitive.usize.html),
    row: &mut [Row](row/struct.Row.html)<'a>,
)

## [Source](../../src/polars_core/frame/row/dataframe.rs.html#42)
#### pub fn [from_rows_and_schema](#method.from_rows_and_schema)(
    rows: &[[Row](row/struct.Row.html)<'_>],
    schema: &Schema<[DataType](../prelude/enum.DataType.html), [()](https://doc.rust-lang.org/nightly/std/primitive.unit.html)>,
) -> [Result](https://doc.rust-lang.org/nightly/core/result/enum.Result.html)<[DataFrame](../prelude/struct.DataFrame.html), [PolarsError](../prelude/enum.PolarsError.html)>

[Source](../../src/polars_core/frame/row/dataframe.rs.html#42)

#### pub fn [from_rows_and_schema](#method.from_rows_and_schema)(
    rows: &[[Row](row/struct.Row.html)<'_>],
    schema: &Schema<[DataType](../prelude/enum.DataType.html), [()](https://doc.rust-lang.org/nightly/std/primitive.unit.html)>,
) -> [Result](https://doc.rust-lang.org/nightly/core/result/enum.Result.html)<[DataFrame](../prelude/struct.DataFrame.html), [PolarsError](../prelude/enum.PolarsError.html)>

## [Source](../../src/polars_core/frame/row/dataframe.rs.html#50-52)
#### pub fn [from_rows_iter_and_schema](#method.from_rows_iter_and_schema)<'a, I>(
    rows: I,
    schema: &Schema<[DataType](../prelude/enum.DataType.html), [()](https://doc.rust-lang.org/nightly/std/primitive.unit.html)>,
) -> [Result](https://doc.rust-lang.org/nightly/core/result/enum.Result.html)<[DataFrame](../prelude/struct.DataFrame.html), [PolarsError](../prelude/enum.PolarsError.html)>

[Source](../../src/polars_core/frame/row/dataframe.rs.html#50-52)

#### pub fn [from_rows_iter_and_schema](#method.from_rows_iter_and_schema)<'a, I>(
    rows: I,
    schema: &Schema<[DataType](../prelude/enum.DataType.html), [()](https://doc.rust-lang.org/nightly/std/primitive.unit.html)>,
) -> [Result](https://doc.rust-lang.org/nightly/core/result/enum.Result.html)<[DataFrame](../prelude/struct.DataFrame.html), [PolarsError](../prelude/enum.PolarsError.html)>

## [Source](../../src/polars_core/frame/row/dataframe.rs.html#100-102)
#### pub fn [try_from_rows_iter_and_schema](#method.try_from_rows_iter_and_schema)<'a, I>(
    rows: I,
    schema: &Schema<[DataType](../prelude/enum.DataType.html), [()](https://doc.rust-lang.org/nightly/std/primitive.unit.html)>,
) -> [Result](https://doc.rust-lang.org/nightly/core/result/enum.Result.html)<[DataFrame](../prelude/struct.DataFrame.html), [PolarsError](../prelude/enum.PolarsError.html)>

[Source](../../src/polars_core/frame/row/dataframe.rs.html#100-102)

#### pub fn [try_from_rows_iter_and_schema](#method.try_from_rows_iter_and_schema)<'a, I>(
    rows: I,
    schema: &Schema<[DataType](../prelude/enum.DataType.html), [()](https://doc.rust-lang.org/nightly/std/primitive.unit.html)>,
) -> [Result](https://doc.rust-lang.org/nightly/core/result/enum.Result.html)<[DataFrame](../prelude/struct.DataFrame.html), [PolarsError](../prelude/enum.PolarsError.html)>

## [Source](../../src/polars_core/frame/row/transpose.rs.html#7)
### impl [DataFrame](../prelude/struct.DataFrame.html)

[Source](../../src/polars_core/frame/row/transpose.rs.html#7)

### impl [DataFrame](../prelude/struct.DataFrame.html)

[Source](../../src/polars_core/frame/row/transpose.rs.html#94-98)

#### pub fn [transpose](#method.transpose)(
    &mut self,
    keep_names_as: [Option](https://doc.rust-lang.org/nightly/core/option/enum.Option.html)<&[str](https://doc.rust-lang.org/nightly/std/primitive.str.html)>,
    new_col_names: [Option](https://doc.rust-lang.org/nightly/core/option/enum.Option.html)<[Either](https://docs.rs/either/1/either/enum.Either.html)<[String](https://doc.rust-lang.org/nightly/alloc/string/struct.String.html), [Vec](https://doc.rust-lang.org/nightly/alloc/vec/struct.Vec.html)<[String](https://doc.rust-lang.org/nightly/alloc/string/struct.String.html)>>>,
) -> [Result](https://doc.rust-lang.org/nightly/core/result/enum.Result.html)<[DataFrame](../prelude/struct.DataFrame.html), [PolarsError](../prelude/enum.PolarsError.html)>

## [Source](../../src/polars_core/frame/row/transpose.rs.html#110-114)
#### pub fn [transpose_impl](#method.transpose_impl)(
    &mut self,
    keep_names_as: [Option](https://doc.rust-lang.org/nightly/core/option/enum.Option.html)<&[str](https://doc.rust-lang.org/nightly/std/primitive.str.html)>,
    new_col_names: [Option](https://doc.rust-lang.org/nightly/core/option/enum.Option.html)<[Either](https://docs.rs/either/1/either/enum.Either.html)<[PlSmallStr](../prelude/struct.PlSmallStr.html), [Vec](https://doc.rust-lang.org/nightly/alloc/vec/struct.Vec.html)<[PlSmallStr](../prelude/struct.PlSmallStr.html)>>>,
) -> [Result](https://doc.rust-lang.org/nightly/core/result/enum.Result.html)<[DataFrame](../prelude/struct.DataFrame.html), [PolarsError](../prelude/enum.PolarsError.html)>

[Source](../../src/polars_core/frame/row/transpose.rs.html#110-114)

#### pub fn [transpose_impl](#method.transpose_impl)(
    &mut self,
    keep_names_as: [Option](https://doc.rust-lang.org/nightly/core/option/enum.Option.html)<&[str](https://doc.rust-lang.org/nightly/std/primitive.str.html)>,
    new_col_names: [Option](https://doc.rust-lang.org/nightly/core/option/enum.Option.html)<[Either](https://docs.rs/either/1/either/enum.Either.html)<[PlSmallStr](../prelude/struct.PlSmallStr.html), [Vec](https://doc.rust-lang.org/nightly/alloc/vec/struct.Vec.html)<[PlSmallStr](../prelude/struct.PlSmallStr.html)>>>,
) -> [Result](https://doc.rust-lang.org/nightly/core/result/enum.Result.html)<[DataFrame](../prelude/struct.DataFrame.html), [PolarsError](../prelude/enum.PolarsError.html)>

Transpose a DataFrame. This is a very expensive operation.

## [Source](../../src/polars_core/frame/mod.rs.html#88)
### impl [DataFrame](../prelude/struct.DataFrame.html)

[Source](../../src/polars_core/frame/mod.rs.html#88)

### impl [DataFrame](../prelude/struct.DataFrame.html)

[Source](../../src/polars_core/frame/mod.rs.html#89)

#### pub fn [materialized_column_iter](#method.materialized_column_iter)(&self) -> impl [ExactSizeIterator](https://doc.rust-lang.org/nightly/core/iter/traits/exact_size/trait.ExactSizeIterator.html)

## [Source](../../src/polars_core/frame/mod.rs.html#105)
#### pub fn [estimated_size](#method.estimated_size)(&self) -> [usize](https://doc.rust-lang.org/nightly/std/primitive.usize.html)

[Source](../../src/polars_core/frame/mod.rs.html#105)

#### pub fn [estimated_size](#method.estimated_size)(&self) -> [usize](https://doc.rust-lang.org/nightly/std/primitive.usize.html)

Returns an estimation of the total (heap) allocated size of the `DataFrame` in bytes.

#####Implementation

This estimation is the sum of the size of its buffers, validity, including nested arrays.
Multiple arrays may share buffers and bitmaps. Therefore, the size of 2 arrays is not the
sum of the sizes computed from this function. In particular, [`StructArray`](../prelude/struct.StructArray.html)’s size is an upper bound.

When an array is sliced, its allocated size remains constant because the buffer unchanged. However, this function will yield a smaller number. This is because this function returns the visible size of the buffer, not its total capacity.

FFI buffers are included in this estimation.

[Source](../../src/polars_core/frame/mod.rs.html#109-112)

#### pub fn [try_apply_columns](#method.try_apply_columns)(
    &self,
    func: impl [Fn](https://doc.rust-lang.org/nightly/core/ops/function/trait.Fn.html)(&[Column](../prelude/enum.Column.html)) -> [Result](https://doc.rust-lang.org/nightly/core/result/enum.Result.html)<[Column](../prelude/enum.Column.html), [PolarsError](../prelude/enum.PolarsError.html)> + [Send](https://doc.rust-lang.org/nightly/core/marker/trait.Send.html) + [Sync](https://doc.rust-lang.org/nightly/core/marker/trait.Sync.html),
) -> [Result](https://doc.rust-lang.org/nightly/core/result/enum.Result.html)<[Vec](https://doc.rust-lang.org/nightly/alloc/vec/struct.Vec.html)<[Column](../prelude/enum.Column.html)>, [PolarsError](../prelude/enum.PolarsError.html)>

[Source](../../src/polars_core/frame/mod.rs.html#123)

#### pub fn [apply_columns](#method.apply_columns)(
    &self,
    func: impl [Fn](https://doc.rust-lang.org/nightly/core/ops/function/trait.Fn.html)(&[Column](../prelude/enum.Column.html)) -> [Column](../prelude/enum.Column.html) + [Send](https://doc.rust-lang.org/nightly/core/marker/trait.Send.html) + [Sync](https://doc.rust-lang.org/nightly/core/marker/trait.Sync.html),
) -> [Vec](https://doc.rust-lang.org/nightly/alloc/vec/struct.Vec.html)<[Column](../prelude/enum.Column.html)>

[Source](../../src/polars_core/frame/mod.rs.html#131-134)

#### pub fn [try_apply_columns_par](#method.try_apply_columns_par)(
    &self,
    func: impl [Fn](https://doc.rust-lang.org/nightly/core/ops/function/trait.Fn.html)(&[Column](../prelude/enum.Column.html)) -> [Result](https://doc.rust-lang.org/nightly/core/result/enum.Result.html)<[Column](../prelude/enum.Column.html), [PolarsError](../prelude/enum.PolarsError.html)> + [Send](https://doc.rust-lang.org/nightly/core/marker/trait.Send.html) + [Sync](https://doc.rust-lang.org/nightly/core/marker/trait.Sync.html),
) -> [Result](https://doc.rust-lang.org/nightly/core/result/enum.Result.html)<[Vec](https://doc.rust-lang.org/nightly/alloc/vec/struct.Vec.html)<[Column](../prelude/enum.Column.html)>, [PolarsError](../prelude/enum.PolarsError.html)>

[Source](../../src/polars_core/frame/mod.rs.html#145)

#### pub fn [apply_columns_par](#method.apply_columns_par)(
    &self,
    func: impl [Fn](https://doc.rust-lang.org/nightly/core/ops/function/trait.Fn.html)(&[Column](../prelude/enum.Column.html)) -> [Column](../prelude/enum.Column.html) + [Send](https://doc.rust-lang.org/nightly/core/marker/trait.Send.html) + [Sync](https://doc.rust-lang.org/nightly/core/marker/trait.Sync.html),
) -> [Vec](https://doc.rust-lang.org/nightly/alloc/vec/struct.Vec.html)<[Column](../prelude/enum.Column.html)>

[Source](../../src/polars_core/frame/mod.rs.html#163)

#### pub fn [new_from_index](#method.new_from_index)(&self, index: [usize](https://doc.rust-lang.org/nightly/std/primitive.usize.html), height: [usize](https://doc.rust-lang.org/nightly/std/primitive.usize.html)) -> [DataFrame](../prelude/struct.DataFrame.html)

## [Source](../../src/polars_core/frame/mod.rs.html#170)
#### pub fn [full_null](#method.full_null)(schema: &Schema<[DataType](../prelude/enum.DataType.html), [()](https://doc.rust-lang.org/nightly/std/primitive.unit.html)>, height: [usize](https://doc.rust-lang.org/nightly/std/primitive.usize.html)) -> [DataFrame](../prelude/struct.DataFrame.html)

[Source](../../src/polars_core/frame/mod.rs.html#170)

#### pub fn [full_null](#method.full_null)(schema: &Schema<[DataType](../prelude/enum.DataType.html), [()](https://doc.rust-lang.org/nightly/std/primitive.unit.html)>, height: [usize](https://doc.rust-lang.org/nightly/std/primitive.usize.html)) -> [DataFrame](../prelude/struct.DataFrame.html)

Create a new `DataFrame` with the given schema, only containing nulls.

## [Source](../../src/polars_core/frame/mod.rs.html#181)
#### pub fn [ensure_matches_schema](#method.ensure_matches_schema)(
    &mut self,
    schema: &Schema<[DataType](../prelude/enum.DataType.html), [()](https://doc.rust-lang.org/nightly/std/primitive.unit.html)>,
) -> [Result](https://doc.rust-lang.org/nightly/core/result/enum.Result.html)<[()](https://doc.rust-lang.org/nightly/std/primitive.unit.html), [PolarsError](../prelude/enum.PolarsError.html)>

[Source](../../src/polars_core/frame/mod.rs.html#181)

#### pub fn [ensure_matches_schema](#method.ensure_matches_schema)(
    &mut self,
    schema: &Schema<[DataType](../prelude/enum.DataType.html), [()](https://doc.rust-lang.org/nightly/std/primitive.unit.html)>,
) -> [Result](https://doc.rust-lang.org/nightly/core/result/enum.Result.html)<[()](https://doc.rust-lang.org/nightly/std/primitive.unit.html), [PolarsError](../prelude/enum.PolarsError.html)>

Ensure this DataFrame matches the given schema. Casts null columns to the expected schema if necessary (but nothing else).

## [Source](../../src/polars_core/frame/mod.rs.html#242)
#### pub fn [with_row_index](#method.with_row_index)(
    &self,
    name: [PlSmallStr](../prelude/struct.PlSmallStr.html),
    offset: [Option](https://doc.rust-lang.org/nightly/core/option/enum.Option.html)<[u32](https://doc.rust-lang.org/nightly/std/primitive.u32.html)>,
) -> [Result](https://doc.rust-lang.org/nightly/core/result/enum.Result.html)<[DataFrame](../prelude/struct.DataFrame.html), [PolarsError](../prelude/enum.PolarsError.html)>

[Source](../../src/polars_core/frame/mod.rs.html#242)

#### pub fn [with_row_index](#method.with_row_index)(
    &self,
    name: [PlSmallStr](../prelude/struct.PlSmallStr.html),
    offset: [Option](https://doc.rust-lang.org/nightly/core/option/enum.Option.html)<[u32](https://doc.rust-lang.org/nightly/std/primitive.u32.html)>,
) -> [Result](https://doc.rust-lang.org/nightly/core/result/enum.Result.html)<[DataFrame](../prelude/struct.DataFrame.html), [PolarsError](../prelude/enum.PolarsError.html)>

Add a new column at index 0 that counts the rows.

#####Example

```
let df1: DataFrame = df!("Name" => ["James", "Mary", "John", "Patricia"])?;
assert_eq!(df1.shape(), (4, 1));
let df2: DataFrame = df1.with_row_index("Id".into(), None)?;
assert_eq!(df2.shape(), (4, 2));
println!("{}", df2);
```
Output:

```
 shape: (4, 2)
 +-----+----------+
 | Id  | Name     |
 | --- | ---      |
 | u32 | str      |
 +=====+==========+
 | 0   | James    |
 +-----+----------+
 | 1   | Mary     |
 +-----+----------+
 | 2   | John     |
 +-----+----------+
 | 3   | Patricia |
 +-----+----------+
```
## [Source](../../src/polars_core/frame/mod.rs.html#264-268)
#### pub unsafe fn [with_row_index_mut](#method.with_row_index_mut)(
    &mut self,
    name: [PlSmallStr](../prelude/struct.PlSmallStr.html),
    offset: [Option](https://doc.rust-lang.org/nightly/core/option/enum.Option.html)<[u32](https://doc.rust-lang.org/nightly/std/primitive.u32.html)>,
) -> &mut [DataFrame](../prelude/struct.DataFrame.html)

[Source](../../src/polars_core/frame/mod.rs.html#264-268)

#### pub unsafe fn [with_row_index_mut](#method.with_row_index_mut)(
    &mut self,
    name: [PlSmallStr](../prelude/struct.PlSmallStr.html),
    offset: [Option](https://doc.rust-lang.org/nightly/core/option/enum.Option.html)<[u32](https://doc.rust-lang.org/nightly/std/primitive.u32.html)>,
) -> &mut [DataFrame](../prelude/struct.DataFrame.html)

## [Source](../../src/polars_core/frame/mod.rs.html#283)
#### pub fn [shrink_to_fit](#method.shrink_to_fit)(&mut self)

[Source](../../src/polars_core/frame/mod.rs.html#283)

#### pub fn [shrink_to_fit](#method.shrink_to_fit)(&mut self)

Shrink the capacity of this DataFrame to fit its length.

## [Source](../../src/polars_core/frame/mod.rs.html#292)
#### pub fn [rechunk_mut_par](#method.rechunk_mut_par)(&mut self) -> &mut [DataFrame](../prelude/struct.DataFrame.html)

[Source](../../src/polars_core/frame/mod.rs.html#292)

#### pub fn [rechunk_mut_par](#method.rechunk_mut_par)(&mut self) -> &mut [DataFrame](../prelude/struct.DataFrame.html)

Aggregate all the chunks in the DataFrame to a single chunk in parallel. This may lead to more peak memory consumption.

## [Source](../../src/polars_core/frame/mod.rs.html#305)
#### pub fn [rechunk_mut](#method.rechunk_mut)(&mut self) -> &mut [DataFrame](../prelude/struct.DataFrame.html)

[Source](../../src/polars_core/frame/mod.rs.html#305)

#### pub fn [rechunk_mut](#method.rechunk_mut)(&mut self) -> &mut [DataFrame](../prelude/struct.DataFrame.html)

Rechunks all columns to only have a single chunk.

## [Source](../../src/polars_core/frame/mod.rs.html#317)
#### pub fn [should_rechunk](#method.should_rechunk)(&self) -> [bool](https://doc.rust-lang.org/nightly/std/primitive.bool.html)

[Source](../../src/polars_core/frame/mod.rs.html#317)

#### pub fn [should_rechunk](#method.should_rechunk)(&self) -> [bool](https://doc.rust-lang.org/nightly/std/primitive.bool.html)

Returns true if the chunks of the columns do not align and re-chunking should be done

## [Source](../../src/polars_core/frame/mod.rs.html#358)
#### pub fn [align_chunks_par](#method.align_chunks_par)(&mut self) -> &mut [DataFrame](../prelude/struct.DataFrame.html)

[Source](../../src/polars_core/frame/mod.rs.html#358)

#### pub fn [align_chunks_par](#method.align_chunks_par)(&mut self) -> &mut [DataFrame](../prelude/struct.DataFrame.html)

Ensure all the chunks in the [`DataFrame`](../prelude/struct.DataFrame.html) are aligned.

## [Source](../../src/polars_core/frame/mod.rs.html#367)
#### pub fn [align_chunks](#method.align_chunks)(&mut self) -> &mut [DataFrame](../prelude/struct.DataFrame.html)

[Source](../../src/polars_core/frame/mod.rs.html#367)

#### pub fn [align_chunks](#method.align_chunks)(&mut self) -> &mut [DataFrame](../prelude/struct.DataFrame.html)

Ensure all the chunks in the [`DataFrame`](../prelude/struct.DataFrame.html) are aligned.

## [Source](../../src/polars_core/frame/mod.rs.html#385)
#### pub fn [get_column_names](#method.get_column_names)(&self) -> [Vec](https://doc.rust-lang.org/nightly/alloc/vec/struct.Vec.html)<&[PlSmallStr](../prelude/struct.PlSmallStr.html)>

[Source](../../src/polars_core/frame/mod.rs.html#385)

#### pub fn [get_column_names](#method.get_column_names)(&self) -> [Vec](https://doc.rust-lang.org/nightly/alloc/vec/struct.Vec.html)<&[PlSmallStr](../prelude/struct.PlSmallStr.html)>

#####Example

```
let df: DataFrame = df!("Language" => ["Rust", "Python"],
                        "Designer" => ["Graydon Hoare", "Guido van Rossum"])?;
assert_eq!(df.get_column_names(), &["Language", "Designer"]);
```
## [Source](../../src/polars_core/frame/mod.rs.html#390)
#### pub fn [get_column_names_owned](#method.get_column_names_owned)(&self) -> [Vec](https://doc.rust-lang.org/nightly/alloc/vec/struct.Vec.html)<[PlSmallStr](../prelude/struct.PlSmallStr.html)>

[Source](../../src/polars_core/frame/mod.rs.html#390)

#### pub fn [get_column_names_owned](#method.get_column_names_owned)(&self) -> [Vec](https://doc.rust-lang.org/nightly/alloc/vec/struct.Vec.html)<[PlSmallStr](../prelude/struct.PlSmallStr.html)>

Get the [`Vec<PlSmallStr>`](https://doc.rust-lang.org/nightly/alloc/vec/struct.Vec.html) representing the column names.

## [Source](../../src/polars_core/frame/mod.rs.html#405-407)
#### pub fn [set_column_names](#method.set_column_names)<T>(
    &mut self,
    new_names: &[\[T\]](https://doc.rust-lang.org/nightly/std/primitive.slice.html),
) -> [Result](https://doc.rust-lang.org/nightly/core/result/enum.Result.html)<[()](https://doc.rust-lang.org/nightly/std/primitive.unit.html), [PolarsError](../prelude/enum.PolarsError.html)>

[Source](../../src/polars_core/frame/mod.rs.html#405-407)

#### pub fn [set_column_names](#method.set_column_names)<T>(
    &mut self,
    new_names: &[\[T\]](https://doc.rust-lang.org/nightly/std/primitive.slice.html),
) -> [Result](https://doc.rust-lang.org/nightly/core/result/enum.Result.html)<[()](https://doc.rust-lang.org/nightly/std/primitive.unit.html), [PolarsError](../prelude/enum.PolarsError.html)>

Set the column names.

#####Example

```
let mut df: DataFrame = df!("Mathematical set" => ["ℕ", "ℤ", "𝔻", "ℚ", "ℝ", "ℂ"])?;
df.set_column_names(&["Set"])?;
assert_eq!(df.get_column_names(), &["Set"]);
```
## [Source](../../src/polars_core/frame/mod.rs.html#443)
#### pub fn [first_col_n_chunks](#method.first_col_n_chunks)(&self) -> [usize](https://doc.rust-lang.org/nightly/std/primitive.usize.html)

[Source](../../src/polars_core/frame/mod.rs.html#443)

#### pub fn [first_col_n_chunks](#method.first_col_n_chunks)(&self) -> [usize](https://doc.rust-lang.org/nightly/std/primitive.usize.html)

The number of chunks for the first column.

## [Source](../../src/polars_core/frame/mod.rs.html#452)
#### pub fn [max_n_chunks](#method.max_n_chunks)(&self) -> [usize](https://doc.rust-lang.org/nightly/std/primitive.usize.html)

[Source](../../src/polars_core/frame/mod.rs.html#452)

#### pub fn [max_n_chunks](#method.max_n_chunks)(&self) -> [usize](https://doc.rust-lang.org/nightly/std/primitive.usize.html)

The highest number of chunks for any column.

## [Source](../../src/polars_core/frame/mod.rs.html#475)
#### pub fn [fields](#method.fields)(&self) -> [Vec](https://doc.rust-lang.org/nightly/alloc/vec/struct.Vec.html)<[Field](../prelude/struct.Field.html)>

[Source](../../src/polars_core/frame/mod.rs.html#475)

#### pub fn [fields](#method.fields)(&self) -> [Vec](https://doc.rust-lang.org/nightly/alloc/vec/struct.Vec.html)<[Field](../prelude/struct.Field.html)>

Generate the schema fields of the [`DataFrame`](../prelude/struct.DataFrame.html).

#####Example

```
let earth: DataFrame = df!("Surface type" => ["Water", "Land"],
                           "Fraction" => [0.708, 0.292])?;
let f1: Field = Field::new("Surface type".into(), DataType::String);
let f2: Field = Field::new("Fraction".into(), DataType::Float64);
assert_eq!(earth.fields(), &[f1, f2]);
```
## [Source](../../src/polars_core/frame/mod.rs.html#515)
#### pub fn [hstack](#method.hstack)(&self, columns: &[[Column](../prelude/enum.Column.html)]) -> [Result](https://doc.rust-lang.org/nightly/core/result/enum.Result.html)<[DataFrame](../prelude/struct.DataFrame.html), [PolarsError](../prelude/enum.PolarsError.html)>

[Source](../../src/polars_core/frame/mod.rs.html#515)

#### pub fn [hstack](#method.hstack)(&self, columns: &[[Column](../prelude/enum.Column.html)]) -> [Result](https://doc.rust-lang.org/nightly/core/result/enum.Result.html)<[DataFrame](../prelude/struct.DataFrame.html), [PolarsError](../prelude/enum.PolarsError.html)>

Add multiple [`Series`](../prelude/struct.Series.html) to a [`DataFrame`](../prelude/struct.DataFrame.html).
The added `Series` are required to have the same length.

#####Example

```
let df1: DataFrame = df!("Element" => ["Copper", "Silver", "Gold"])?;
let s1 = Column::new("Proton".into(), [29, 47, 79]);
let s2 = Column::new("Electron".into(), [29, 47, 79]);
let df2: DataFrame = df1.hstack(&[s1, s2])?;
assert_eq!(df2.shape(), (3, 3));
println!("{}", df2);
```
Output:

```
shape: (3, 3)
+---------+--------+----------+
| Element | Proton | Electron |
| ---     | ---    | ---      |
| str     | i32    | i32      |
+=========+========+==========+
| Copper  | 29     | 29       |
+---------+--------+----------+
| Silver  | 47     | 47       |
+---------+--------+----------+
| Gold    | 79     | 79       |
+---------+--------+----------+
```
## [Source](../../src/polars_core/frame/mod.rs.html#563)
#### pub fn [vstack](#method.vstack)(&self, other: &[DataFrame](../prelude/struct.DataFrame.html)) -> [Result](https://doc.rust-lang.org/nightly/core/result/enum.Result.html)<[DataFrame](../prelude/struct.DataFrame.html), [PolarsError](../prelude/enum.PolarsError.html)>

[Source](../../src/polars_core/frame/mod.rs.html#563)

#### pub fn [vstack](#method.vstack)(&self, other: &[DataFrame](../prelude/struct.DataFrame.html)) -> [Result](https://doc.rust-lang.org/nightly/core/result/enum.Result.html)<[DataFrame](../prelude/struct.DataFrame.html), [PolarsError](../prelude/enum.PolarsError.html)>

Concatenate a [`DataFrame`](../prelude/struct.DataFrame.html) to this [`DataFrame`](../prelude/struct.DataFrame.html) and return as newly allocated [`DataFrame`](../prelude/struct.DataFrame.html).

If many `vstack` operations are done, it is recommended to call [`DataFrame::align_chunks_par`](../prelude/struct.DataFrame.html#method.align_chunks_par).

#####Example

```
let df1: DataFrame = df!("Element" => ["Copper", "Silver", "Gold"],
                         "Melting Point (K)" => [1357.77, 1234.93, 1337.33])?;
let df2: DataFrame = df!("Element" => ["Platinum", "Palladium"],
                         "Melting Point (K)" => [2041.4, 1828.05])?;
let df3: DataFrame = df1.vstack(&df2)?;
assert_eq!(df3.shape(), (5, 2));
println!("{}", df3);
```
Output:

```
shape: (5, 2)
+-----------+-------------------+
| Element   | Melting Point (K) |
| ---       | ---               |
| str       | f64               |
+===========+===================+
| Copper    | 1357.77           |
+-----------+-------------------+
| Silver    | 1234.93           |
+-----------+-------------------+
| Gold      | 1337.33           |
+-----------+-------------------+
| Platinum  | 2041.4            |
+-----------+-------------------+
| Palladium | 1828.05           |
+-----------+-------------------+
```
## [Source](../../src/polars_core/frame/mod.rs.html#609)
#### pub fn [vstack_mut](#method.vstack_mut)(
    &mut self,
    other: &[DataFrame](../prelude/struct.DataFrame.html),
) -> [Result](https://doc.rust-lang.org/nightly/core/result/enum.Result.html)<&mut [DataFrame](../prelude/struct.DataFrame.html), [PolarsError](../prelude/enum.PolarsError.html)>

[Source](../../src/polars_core/frame/mod.rs.html#609)

#### pub fn [vstack_mut](#method.vstack_mut)(
    &mut self,
    other: &[DataFrame](../prelude/struct.DataFrame.html),
) -> [Result](https://doc.rust-lang.org/nightly/core/result/enum.Result.html)<&mut [DataFrame](../prelude/struct.DataFrame.html), [PolarsError](../prelude/enum.PolarsError.html)>

Concatenate a [`DataFrame`](../prelude/struct.DataFrame.html) to this `DataFrame`

If many `vstack` operations are done, it is recommended to call [`DataFrame::align_chunks_par`](../prelude/struct.DataFrame.html#method.align_chunks_par).

#####Example

```
let mut df1: DataFrame = df!("Element" => ["Copper", "Silver", "Gold"],
                         "Melting Point (K)" => [1357.77, 1234.93, 1337.33])?;
let df2: DataFrame = df!("Element" => ["Platinum", "Palladium"],
                         "Melting Point (K)" => [2041.4, 1828.05])?;
df1.vstack_mut(&df2)?;
assert_eq!(df1.shape(), (5, 2));
println!("{}", df1);
```
Output:

```
shape: (5, 2)
+-----------+-------------------+
| Element   | Melting Point (K) |
| ---       | ---               |
| str       | f64               |
+===========+===================+
| Copper    | 1357.77           |
+-----------+-------------------+
| Silver    | 1234.93           |
+-----------+-------------------+
| Gold      | 1337.33           |
+-----------+-------------------+
| Platinum  | 2041.4            |
+-----------+-------------------+
| Palladium | 1828.05           |
+-----------+-------------------+
```
[Source](../../src/polars_core/frame/mod.rs.html#640)

#### pub fn [vstack_mut_owned](#method.vstack_mut_owned)(
    &mut self,
    other: [DataFrame](../prelude/struct.DataFrame.html),
) -> [Result](https://doc.rust-lang.org/nightly/core/result/enum.Result.html)<&mut [DataFrame](../prelude/struct.DataFrame.html), [PolarsError](../prelude/enum.PolarsError.html)>

## [Source](../../src/polars_core/frame/mod.rs.html#678)
#### pub fn [vstack_mut_unchecked](#method.vstack_mut_unchecked)(&mut self, other: &[DataFrame](../prelude/struct.DataFrame.html)) -> &mut [DataFrame](../prelude/struct.DataFrame.html)

[Source](../../src/polars_core/frame/mod.rs.html#678)

#### pub fn [vstack_mut_unchecked](#method.vstack_mut_unchecked)(&mut self, other: &[DataFrame](../prelude/struct.DataFrame.html)) -> &mut [DataFrame](../prelude/struct.DataFrame.html)

Concatenate a [`DataFrame`](../prelude/struct.DataFrame.html) to this `DataFrame`

If many `vstack` operations are done, it is recommended to call [`DataFrame::align_chunks_par`](../prelude/struct.DataFrame.html#method.align_chunks_par).

#####Panics

Panics if the schema’s don’t match.

## [Source](../../src/polars_core/frame/mod.rs.html#701)
#### pub fn [vstack_mut_owned_unchecked](#method.vstack_mut_owned_unchecked)(&mut self, other: [DataFrame](../prelude/struct.DataFrame.html)) -> &mut [DataFrame](../prelude/struct.DataFrame.html)

[Source](../../src/polars_core/frame/mod.rs.html#701)

#### pub fn [vstack_mut_owned_unchecked](#method.vstack_mut_owned_unchecked)(&mut self, other: [DataFrame](../prelude/struct.DataFrame.html)) -> &mut [DataFrame](../prelude/struct.DataFrame.html)

Concatenate a [`DataFrame`](../prelude/struct.DataFrame.html) to this `DataFrame`

If many `vstack` operations are done, it is recommended to call [`DataFrame::align_chunks_par`](../prelude/struct.DataFrame.html#method.align_chunks_par).

#####Panics

Panics if the schema’s don’t match.

## [Source](../../src/polars_core/frame/mod.rs.html#730)
#### pub fn [extend](#method.extend)(&mut self, other: &[DataFrame](../prelude/struct.DataFrame.html)) -> [Result](https://doc.rust-lang.org/nightly/core/result/enum.Result.html)<[()](https://doc.rust-lang.org/nightly/std/primitive.unit.html), [PolarsError](../prelude/enum.PolarsError.html)>

[Source](../../src/polars_core/frame/mod.rs.html#730)

#### pub fn [extend](#method.extend)(&mut self, other: &[DataFrame](../prelude/struct.DataFrame.html)) -> [Result](https://doc.rust-lang.org/nightly/core/result/enum.Result.html)<[()](https://doc.rust-lang.org/nightly/std/primitive.unit.html), [PolarsError](../prelude/enum.PolarsError.html)>

Extend the memory backed by this [`DataFrame`](../prelude/struct.DataFrame.html) with the values from `other`.

Different from [`vstack`](../prelude/struct.DataFrame.html#method.vstack) which adds the chunks from `other` to the chunks of this `DataFrame``extend` appends the data from `other` to the underlying memory locations and thus may cause a reallocation.

If this does not cause a reallocation, the resulting data structure will not have any extra chunks and thus will yield faster queries.

Prefer `extend` over `vstack` when you want to do a query after a single append. For instance during
online operations where you add `n` rows and rerun a query.

Prefer `vstack` over `extend` when you want to append many times before doing a query. For instance
when you read in multiple files and when to store them in a single `DataFrame`. In the latter case, finish the sequence
of `append` operations with a [`rechunk`](../prelude/struct.DataFrame.html#method.align_chunks_par).

## [Source](../../src/polars_core/frame/mod.rs.html#771)
#### pub fn [drop_in_place](#method.drop_in_place)(&mut self, name: &[str](https://doc.rust-lang.org/nightly/std/primitive.str.html)) -> [Result](https://doc.rust-lang.org/nightly/core/result/enum.Result.html)<[Column](../prelude/enum.Column.html), [PolarsError](../prelude/enum.PolarsError.html)>

[Source](../../src/polars_core/frame/mod.rs.html#771)

#### pub fn [drop_in_place](#method.drop_in_place)(&mut self, name: &[str](https://doc.rust-lang.org/nightly/std/primitive.str.html)) -> [Result](https://doc.rust-lang.org/nightly/core/result/enum.Result.html)<[Column](../prelude/enum.Column.html), [PolarsError](../prelude/enum.PolarsError.html)>

Remove a column by name and return the column removed.

#####Example

```
let mut df: DataFrame = df!("Animal" => ["Tiger", "Lion", "Great auk"],
                            "IUCN" => ["Endangered", "Vulnerable", "Extinct"])?;
let s1: PolarsResult<Column> = df.drop_in_place("Average weight");
assert!(s1.is_err());
let s2: Column = df.drop_in_place("Animal")?;
assert_eq!(s2, Column::new("Animal".into(), &["Tiger", "Lion", "Great auk"]));
```
## [Source](../../src/polars_core/frame/mod.rs.html#804-806)
#### pub fn [drop_nulls](#method.drop_nulls)<S>(
    &self,
    subset: [Option](https://doc.rust-lang.org/nightly/core/option/enum.Option.html)<&[\[S\]](https://doc.rust-lang.org/nightly/std/primitive.slice.html)>,
) -> [Result](https://doc.rust-lang.org/nightly/core/result/enum.Result.html)<[DataFrame](../prelude/struct.DataFrame.html), [PolarsError](../prelude/enum.PolarsError.html)>

[Source](../../src/polars_core/frame/mod.rs.html#804-806)

#### pub fn [drop_nulls](#method.drop_nulls)<S>(
    &self,
    subset: [Option](https://doc.rust-lang.org/nightly/core/option/enum.Option.html)<&[\[S\]](https://doc.rust-lang.org/nightly/std/primitive.slice.html)>,
) -> [Result](https://doc.rust-lang.org/nightly/core/result/enum.Result.html)<[DataFrame](../prelude/struct.DataFrame.html), [PolarsError](../prelude/enum.PolarsError.html)>

Return a new [`DataFrame`](../prelude/struct.DataFrame.html) where all null values are dropped.

#####Example

```
let df1: DataFrame = df!("Country" => ["Malta", "Liechtenstein", "North Korea"],
                        "Tax revenue (% GDP)" => [Some(32.7), None, None])?;
assert_eq!(df1.shape(), (3, 2));
let df2: DataFrame = df1.drop_nulls::<String>(None)?;
assert_eq!(df2.shape(), (1, 2));
println!("{}", df2);
```
Output:

```
shape: (1, 2)
+---------+---------------------+
| Country | Tax revenue (% GDP) |
| ---     | ---                 |
| str     | f64                 |
+=========+=====================+
| Malta   | 32.7                |
+---------+---------------------+
```
## [Source](../../src/polars_core/frame/mod.rs.html#873)
#### pub fn [drop_many_amortized](#method.drop_many_amortized)(
    &self,
    names: &HashSet<[PlSmallStr](../prelude/struct.PlSmallStr.html), RandomState>,
) -> [DataFrame](../prelude/struct.DataFrame.html)

[Source](../../src/polars_core/frame/mod.rs.html#873)

#### pub fn [drop_many_amortized](#method.drop_many_amortized)(
    &self,
    names: &HashSet<[PlSmallStr](../prelude/struct.PlSmallStr.html), RandomState>,
) -> [DataFrame](../prelude/struct.DataFrame.html)

Drop columns that are in `names` without allocating a [`HashSet`](https://doc.rust-lang.org/nightly/std/collections/hash/set/struct.HashSet.html).

## [Source](../../src/polars_core/frame/mod.rs.html#910)
#### pub fn [insert_column](#method.insert_column)(
    &mut self,
    index: [usize](https://doc.rust-lang.org/nightly/std/primitive.usize.html),
    column: [Column](../prelude/enum.Column.html),
) -> [Result](https://doc.rust-lang.org/nightly/core/result/enum.Result.html)<&mut [DataFrame](../prelude/struct.DataFrame.html), [PolarsError](../prelude/enum.PolarsError.html)>

[Source](../../src/polars_core/frame/mod.rs.html#910)

#### pub fn [insert_column](#method.insert_column)(
    &mut self,
    index: [usize](https://doc.rust-lang.org/nightly/std/primitive.usize.html),
    column: [Column](../prelude/enum.Column.html),
) -> [Result](https://doc.rust-lang.org/nightly/core/result/enum.Result.html)<&mut [DataFrame](../prelude/struct.DataFrame.html), [PolarsError](../prelude/enum.PolarsError.html)>

Insert a new column at a given index.

## [Source](../../src/polars_core/frame/mod.rs.html#924)
#### pub fn [with_column](#method.with_column)(
    &mut self,
    column: [Column](../prelude/enum.Column.html),
) -> [Result](https://doc.rust-lang.org/nightly/core/result/enum.Result.html)<&mut [DataFrame](../prelude/struct.DataFrame.html), [PolarsError](../prelude/enum.PolarsError.html)>

[Source](../../src/polars_core/frame/mod.rs.html#924)

#### pub fn [with_column](#method.with_column)(
    &mut self,
    column: [Column](../prelude/enum.Column.html),
) -> [Result](https://doc.rust-lang.org/nightly/core/result/enum.Result.html)<&mut [DataFrame](../prelude/struct.DataFrame.html), [PolarsError](../prelude/enum.PolarsError.html)>

Add a new column to this [`DataFrame`](../prelude/struct.DataFrame.html) or replace an existing one. Broadcasts unit-length
columns.

## [Source](../../src/polars_core/frame/mod.rs.html#945)
#### pub unsafe fn [push_column_unchecked](#method.push_column_unchecked)(&mut self, column: [Column](../prelude/enum.Column.html)) -> &mut [DataFrame](../prelude/struct.DataFrame.html)

[Source](../../src/polars_core/frame/mod.rs.html#945)

#### pub unsafe fn [push_column_unchecked](#method.push_column_unchecked)(&mut self, column: [Column](../prelude/enum.Column.html)) -> &mut [DataFrame](../prelude/struct.DataFrame.html)

## [Source](../../src/polars_core/frame/mod.rs.html#952-956)
#### pub fn [with_columns_mut](#method.with_columns_mut)(
    &mut self,
    columns: impl [IntoIterator](https://doc.rust-lang.org/nightly/core/iter/traits/collect/trait.IntoIterator.html)<Item = [Column](../prelude/enum.Column.html)>,
    output_schema: &Schema<[DataType](../prelude/enum.DataType.html), [()](https://doc.rust-lang.org/nightly/std/primitive.unit.html)>,
) -> [Result](https://doc.rust-lang.org/nightly/core/result/enum.Result.html)<[()](https://doc.rust-lang.org/nightly/std/primitive.unit.html), [PolarsError](../prelude/enum.PolarsError.html)>

[Source](../../src/polars_core/frame/mod.rs.html#952-956)

#### pub fn [with_columns_mut](#method.with_columns_mut)(
    &mut self,
    columns: impl [IntoIterator](https://doc.rust-lang.org/nightly/core/iter/traits/collect/trait.IntoIterator.html)<Item = [Column](../prelude/enum.Column.html)>,
    output_schema: &Schema<[DataType](../prelude/enum.DataType.html), [()](https://doc.rust-lang.org/nightly/std/primitive.unit.html)>,
) -> [Result](https://doc.rust-lang.org/nightly/core/result/enum.Result.html)<[()](https://doc.rust-lang.org/nightly/std/primitive.unit.html), [PolarsError](../prelude/enum.PolarsError.html)>

Add or replace columns to this [`DataFrame`](../prelude/struct.DataFrame.html) or replace an existing one.
Broadcasts unit-length columns, and uses an existing schema to amortize lookups.

## [Source](../../src/polars_core/frame/mod.rs.html#1028)
#### pub fn [select_at_idx](#method.select_at_idx)(&self, idx: [usize](https://doc.rust-lang.org/nightly/std/primitive.usize.html)) -> [Option](https://doc.rust-lang.org/nightly/core/option/enum.Option.html)<&[Column](../prelude/enum.Column.html)>

[Source](../../src/polars_core/frame/mod.rs.html#1028)

#### pub fn [select_at_idx](#method.select_at_idx)(&self, idx: [usize](https://doc.rust-lang.org/nightly/std/primitive.usize.html)) -> [Option](https://doc.rust-lang.org/nightly/core/option/enum.Option.html)<&[Column](../prelude/enum.Column.html)>

Select a [`Series`](../prelude/struct.Series.html) by index.

#####Example

```
let df: DataFrame = df!("Star" => ["Sun", "Betelgeuse", "Sirius A", "Sirius B"],
                        "Absolute magnitude" => [4.83, -5.85, 1.42, 11.18])?;
let s1: Option<&Column> = df.select_at_idx(0);
let s2 = Column::new("Star".into(), ["Sun", "Betelgeuse", "Sirius A", "Sirius B"]);
assert_eq!(s1, Some(&s2));
```
## [Source](../../src/polars_core/frame/mod.rs.html#1049)
#### pub fn [get_column_index](#method.get_column_index)(&self, name: &[str](https://doc.rust-lang.org/nightly/std/primitive.str.html)) -> [Option](https://doc.rust-lang.org/nightly/core/option/enum.Option.html)<[usize](https://doc.rust-lang.org/nightly/std/primitive.usize.html)>

[Source](../../src/polars_core/frame/mod.rs.html#1049)

#### pub fn [get_column_index](#method.get_column_index)(&self, name: &[str](https://doc.rust-lang.org/nightly/std/primitive.str.html)) -> [Option](https://doc.rust-lang.org/nightly/core/option/enum.Option.html)<[usize](https://doc.rust-lang.org/nightly/std/primitive.usize.html)>

Get column index of a [`Series`](../prelude/struct.Series.html) by name.

#####Example

```
let df: DataFrame = df!("Name" => ["Player 1", "Player 2", "Player 3"],
                        "Health" => [100, 200, 500],
                        "Mana" => [250, 100, 0],
                        "Strength" => [30, 150, 300])?;
assert_eq!(df.get_column_index("Name"), Some(0));
assert_eq!(df.get_column_index("Health"), Some(1));
assert_eq!(df.get_column_index("Mana"), Some(2));
assert_eq!(df.get_column_index("Strength"), Some(3));
assert_eq!(df.get_column_index("Haste"), None);
```
## [Source](../../src/polars_core/frame/mod.rs.html#1060)
#### pub fn [try_get_column_index](#method.try_get_column_index)(&self, name: &[str](https://doc.rust-lang.org/nightly/std/primitive.str.html)) -> [Result](https://doc.rust-lang.org/nightly/core/result/enum.Result.html)<[usize](https://doc.rust-lang.org/nightly/std/primitive.usize.html), [PolarsError](../prelude/enum.PolarsError.html)>

[Source](../../src/polars_core/frame/mod.rs.html#1060)

#### pub fn [try_get_column_index](#method.try_get_column_index)(&self, name: &[str](https://doc.rust-lang.org/nightly/std/primitive.str.html)) -> [Result](https://doc.rust-lang.org/nightly/core/result/enum.Result.html)<[usize](https://doc.rust-lang.org/nightly/std/primitive.usize.html), [PolarsError](../prelude/enum.PolarsError.html)>

Get column index of a [`Series`](../prelude/struct.Series.html) by name.

## [Source](../../src/polars_core/frame/mod.rs.html#1078)
#### pub fn [column](#method.column)(&self, name: &[str](https://doc.rust-lang.org/nightly/std/primitive.str.html)) -> [Result](https://doc.rust-lang.org/nightly/core/result/enum.Result.html)<&[Column](../prelude/enum.Column.html), [PolarsError](../prelude/enum.PolarsError.html)>

[Source](../../src/polars_core/frame/mod.rs.html#1078)

#### pub fn [column](#method.column)(&self, name: &[str](https://doc.rust-lang.org/nightly/std/primitive.str.html)) -> [Result](https://doc.rust-lang.org/nightly/core/result/enum.Result.html)<&[Column](../prelude/enum.Column.html), [PolarsError](../prelude/enum.PolarsError.html)>

Select a single column by name.

#####Example

```
let s1 = Column::new("Password".into(), ["123456", "[]B$u$g$s$B#u#n#n#y[]{}"]);
let s2 = Column::new("Robustness".into(), ["Weak", "Strong"]);
let df: DataFrame = DataFrame::new_infer_height(vec![s1.clone(), s2])?;
assert_eq!(df.column("Password")?, &s1);
```
## [Source](../../src/polars_core/frame/mod.rs.html#1093-1096)
#### pub fn [select](#method.select)<I, S>(&self, names: I) -> [Result](https://doc.rust-lang.org/nightly/core/result/enum.Result.html)<[DataFrame](../prelude/struct.DataFrame.html), [PolarsError](../prelude/enum.PolarsError.html)>

[Source](../../src/polars_core/frame/mod.rs.html#1093-1096)

#### pub fn [select](#method.select)<I, S>(&self, names: I) -> [Result](https://doc.rust-lang.org/nightly/core/result/enum.Result.html)<[DataFrame](../prelude/struct.DataFrame.html), [PolarsError](../prelude/enum.PolarsError.html)>

## [Source](../../src/polars_core/frame/mod.rs.html#1105-1108)
#### pub unsafe fn [select_unchecked](#method.select_unchecked)<I, S>(
    &self,
    names: I,
) -> [Result](https://doc.rust-lang.org/nightly/core/result/enum.Result.html)<[DataFrame](../prelude/struct.DataFrame.html), [PolarsError](../prelude/enum.PolarsError.html)>

[Source](../../src/polars_core/frame/mod.rs.html#1105-1108)

#### pub unsafe fn [select_unchecked](#method.select_unchecked)<I, S>(
    &self,
    names: I,
) -> [Result](https://doc.rust-lang.org/nightly/core/result/enum.Result.html)<[DataFrame](../prelude/struct.DataFrame.html), [PolarsError](../prelude/enum.PolarsError.html)>

## [Source](../../src/polars_core/frame/mod.rs.html#1130-1133)
#### pub fn [select_to_vec](#method.select_to_vec)(
    &self,
    selection: impl [IntoIterator](https://doc.rust-lang.org/nightly/core/iter/traits/collect/trait.IntoIterator.html)<Item = impl [AsRef](https://doc.rust-lang.org/nightly/core/convert/trait.AsRef.html)<[str](https://doc.rust-lang.org/nightly/std/primitive.str.html)>>,
) -> [Result](https://doc.rust-lang.org/nightly/core/result/enum.Result.html)<[Vec](https://doc.rust-lang.org/nightly/alloc/vec/struct.Vec.html)<[Column](../prelude/enum.Column.html)>, [PolarsError](../prelude/enum.PolarsError.html)>

[Source](../../src/polars_core/frame/mod.rs.html#1130-1133)

#### pub fn [select_to_vec](#method.select_to_vec)(
    &self,
    selection: impl [IntoIterator](https://doc.rust-lang.org/nightly/core/iter/traits/collect/trait.IntoIterator.html)<Item = impl [AsRef](https://doc.rust-lang.org/nightly/core/convert/trait.AsRef.html)<[str](https://doc.rust-lang.org/nightly/std/primitive.str.html)>>,
) -> [Result](https://doc.rust-lang.org/nightly/core/result/enum.Result.html)<[Vec](https://doc.rust-lang.org/nightly/alloc/vec/struct.Vec.html)<[Column](../prelude/enum.Column.html)>, [PolarsError](../prelude/enum.PolarsError.html)>

Select column(s) from this [`DataFrame`](../prelude/struct.DataFrame.html) and return them into a [`Vec`](https://doc.rust-lang.org/nightly/alloc/vec/struct.Vec.html).

This does not error on duplicate selections.

#####Example

```
let df: DataFrame = df!("Name" => ["Methane", "Ethane", "Propane"],
                        "Carbon" => [1, 2, 3],
                        "Hydrogen" => [4, 6, 8])?;
let sv: Vec<Column> = df.select_to_vec(["Carbon", "Hydrogen"])?;
assert_eq!(df["Carbon"], sv[0]);
assert_eq!(df["Hydrogen"], sv[1]);
```
## [Source](../../src/polars_core/frame/mod.rs.html#1148)
#### pub fn [filter](#method.filter)(
    &self,
    mask: &[ChunkedArray](../prelude/struct.ChunkedArray.html)<[BooleanType](../prelude/struct.BooleanType.html)>,
) -> [Result](https://doc.rust-lang.org/nightly/core/result/enum.Result.html)<[DataFrame](../prelude/struct.DataFrame.html), [PolarsError](../prelude/enum.PolarsError.html)>

[Source](../../src/polars_core/frame/mod.rs.html#1148)

#### pub fn [filter](#method.filter)(
    &self,
    mask: &[ChunkedArray](../prelude/struct.ChunkedArray.html)<[BooleanType](../prelude/struct.BooleanType.html)>,
) -> [Result](https://doc.rust-lang.org/nightly/core/result/enum.Result.html)<[DataFrame](../prelude/struct.DataFrame.html), [PolarsError](../prelude/enum.PolarsError.html)>

## [Source](../../src/polars_core/frame/mod.rs.html#1183)
#### pub fn [filter_seq](#method.filter_seq)(
    &self,
    mask: &[ChunkedArray](../prelude/struct.ChunkedArray.html)<[BooleanType](../prelude/struct.BooleanType.html)>,
) -> [Result](https://doc.rust-lang.org/nightly/core/result/enum.Result.html)<[DataFrame](../prelude/struct.DataFrame.html), [PolarsError](../prelude/enum.PolarsError.html)>

[Source](../../src/polars_core/frame/mod.rs.html#1183)

#### pub fn [filter_seq](#method.filter_seq)(
    &self,
    mask: &[ChunkedArray](../prelude/struct.ChunkedArray.html)<[BooleanType](../prelude/struct.BooleanType.html)>,
) -> [Result](https://doc.rust-lang.org/nightly/core/result/enum.Result.html)<[DataFrame](../prelude/struct.DataFrame.html), [PolarsError](../prelude/enum.PolarsError.html)>

Same as `filter` but does not parallelize.

## [Source](../../src/polars_core/frame/mod.rs.html#1225)
#### pub fn [take](#method.take)(
    &self,
    indices: &[ChunkedArray](../prelude/struct.ChunkedArray.html)<[UInt32Type](../prelude/struct.UInt32Type.html)>,
) -> [Result](https://doc.rust-lang.org/nightly/core/result/enum.Result.html)<[DataFrame](../prelude/struct.DataFrame.html), [PolarsError](../prelude/enum.PolarsError.html)>

[Source](../../src/polars_core/frame/mod.rs.html#1225)

#### pub fn [take](#method.take)(
    &self,
    indices: &[ChunkedArray](../prelude/struct.ChunkedArray.html)<[UInt32Type](../prelude/struct.UInt32Type.html)>,
) -> [Result](https://doc.rust-lang.org/nightly/core/result/enum.Result.html)<[DataFrame](../prelude/struct.DataFrame.html), [PolarsError](../prelude/enum.PolarsError.html)>

## [Source](../../src/polars_core/frame/mod.rs.html#1238)
#### pub unsafe fn [take_unchecked](#method.take_unchecked)(&self, idx: &[ChunkedArray](../prelude/struct.ChunkedArray.html)<[UInt32Type](../prelude/struct.UInt32Type.html)>) -> [DataFrame](../prelude/struct.DataFrame.html)

[Source](../../src/polars_core/frame/mod.rs.html#1238)

#### pub unsafe fn [take_unchecked](#method.take_unchecked)(&self, idx: &[ChunkedArray](../prelude/struct.ChunkedArray.html)<[UInt32Type](../prelude/struct.UInt32Type.html)>) -> [DataFrame](../prelude/struct.DataFrame.html)

#####Safety

The indices must be in-bounds.

## [Source](../../src/polars_core/frame/mod.rs.html#1245)
#### pub unsafe fn [gather_group_unchecked](#method.gather_group_unchecked)(
    &self,
    group: &[GroupsIndicator](../prelude/enum.GroupsIndicator.html)<'_>,
) -> [DataFrame](../prelude/struct.DataFrame.html)

**crate feature `algorithm_group_by`** only.

[Source](../../src/polars_core/frame/mod.rs.html#1245)

#### pub unsafe fn [gather_group_unchecked](#method.gather_group_unchecked)(
    &self,
    group: &[GroupsIndicator](../prelude/enum.GroupsIndicator.html)<'_>,
) -> [DataFrame](../prelude/struct.DataFrame.html)

**crate feature**only.

`algorithm_group_by`
#####Safety

The indices must be in-bounds.

## [Source](../../src/polars_core/frame/mod.rs.html#1256)
#### pub unsafe fn [take_unchecked_impl](#method.take_unchecked_impl)(
    &self,
    idx: &[ChunkedArray](../prelude/struct.ChunkedArray.html)<[UInt32Type](../prelude/struct.UInt32Type.html)>,
    allow_threads: [bool](https://doc.rust-lang.org/nightly/std/primitive.bool.html),
) -> [DataFrame](../prelude/struct.DataFrame.html)

[Source](../../src/polars_core/frame/mod.rs.html#1256)

#### pub unsafe fn [take_unchecked_impl](#method.take_unchecked_impl)(
    &self,
    idx: &[ChunkedArray](../prelude/struct.ChunkedArray.html)<[UInt32Type](../prelude/struct.UInt32Type.html)>,
    allow_threads: [bool](https://doc.rust-lang.org/nightly/std/primitive.bool.html),
) -> [DataFrame](../prelude/struct.DataFrame.html)

#####Safety

The indices must be in-bounds.

## [Source](../../src/polars_core/frame/mod.rs.html#1298)
#### pub unsafe fn [take_slice_unchecked](#method.take_slice_unchecked)(&self, idx: &[[u32](https://doc.rust-lang.org/nightly/std/primitive.u32.html)]) -> [DataFrame](../prelude/struct.DataFrame.html)

[Source](../../src/polars_core/frame/mod.rs.html#1298)

#### pub unsafe fn [take_slice_unchecked](#method.take_slice_unchecked)(&self, idx: &[[u32](https://doc.rust-lang.org/nightly/std/primitive.u32.html)]) -> [DataFrame](../prelude/struct.DataFrame.html)

#####Safety

The indices must be in-bounds.

## [Source](../../src/polars_core/frame/mod.rs.html#1304)
#### pub unsafe fn [take_slice_unchecked_impl](#method.take_slice_unchecked_impl)(
    &self,
    idx: &[[u32](https://doc.rust-lang.org/nightly/std/primitive.u32.html)],
    allow_threads: [bool](https://doc.rust-lang.org/nightly/std/primitive.bool.html),
) -> [DataFrame](../prelude/struct.DataFrame.html)

[Source](../../src/polars_core/frame/mod.rs.html#1304)

#### pub unsafe fn [take_slice_unchecked_impl](#method.take_slice_unchecked_impl)(
    &self,
    idx: &[[u32](https://doc.rust-lang.org/nightly/std/primitive.u32.html)],
    allow_threads: [bool](https://doc.rust-lang.org/nightly/std/primitive.bool.html),
) -> [DataFrame](../prelude/struct.DataFrame.html)

#####Safety

The indices must be in-bounds.

## [Source](../../src/polars_core/frame/mod.rs.html#1361)
#### pub fn [rename](#method.rename)(
    &mut self,
    column: &[str](https://doc.rust-lang.org/nightly/std/primitive.str.html),
    name: [PlSmallStr](../prelude/struct.PlSmallStr.html),
) -> [Result](https://doc.rust-lang.org/nightly/core/result/enum.Result.html)<&mut [DataFrame](../prelude/struct.DataFrame.html), [PolarsError](../prelude/enum.PolarsError.html)>

[Source](../../src/polars_core/frame/mod.rs.html#1361)

#### pub fn [rename](#method.rename)(
    &mut self,
    column: &[str](https://doc.rust-lang.org/nightly/std/primitive.str.html),
    name: [PlSmallStr](../prelude/struct.PlSmallStr.html),
) -> [Result](https://doc.rust-lang.org/nightly/core/result/enum.Result.html)<&mut [DataFrame](../prelude/struct.DataFrame.html), [PolarsError](../prelude/enum.PolarsError.html)>

[Source](../../src/polars_core/frame/mod.rs.html#1378-1381)

#### pub fn [rename_many](#method.rename_many)<'a>(
    self,
    renames: impl [Iterator](https://doc.rust-lang.org/nightly/core/iter/traits/iterator/trait.Iterator.html)<Item = (&'a [str](https://doc.rust-lang.org/nightly/std/primitive.str.html), [PlSmallStr](../prelude/struct.PlSmallStr.html))>,
) -> [Result](https://doc.rust-lang.org/nightly/core/result/enum.Result.html)<[DataFrame](../prelude/struct.DataFrame.html), [PolarsError](../prelude/enum.PolarsError.html)>

## [Source](../../src/polars_core/frame/mod.rs.html#1415-1419)
#### pub fn [sort_in_place](#method.sort_in_place)(
    &mut self,
    by: impl [IntoIterator](https://doc.rust-lang.org/nightly/core/iter/traits/collect/trait.IntoIterator.html)<Item = impl [AsRef](https://doc.rust-lang.org/nightly/core/convert/trait.AsRef.html)<[str](https://doc.rust-lang.org/nightly/std/primitive.str.html)>>,
    sort_options: [SortMultipleOptions](../prelude/struct.SortMultipleOptions.html),
) -> [Result](https://doc.rust-lang.org/nightly/core/result/enum.Result.html)<&mut [DataFrame](../prelude/struct.DataFrame.html), [PolarsError](../prelude/enum.PolarsError.html)>

[Source](../../src/polars_core/frame/mod.rs.html#1415-1419)

#### pub fn [sort_in_place](#method.sort_in_place)(
    &mut self,
    by: impl [IntoIterator](https://doc.rust-lang.org/nightly/core/iter/traits/collect/trait.IntoIterator.html)<Item = impl [AsRef](https://doc.rust-lang.org/nightly/core/convert/trait.AsRef.html)<[str](https://doc.rust-lang.org/nightly/std/primitive.str.html)>>,
    sort_options: [SortMultipleOptions](../prelude/struct.SortMultipleOptions.html),
) -> [Result](https://doc.rust-lang.org/nightly/core/result/enum.Result.html)<&mut [DataFrame](../prelude/struct.DataFrame.html), [PolarsError](../prelude/enum.PolarsError.html)>

Sort [`DataFrame`](../prelude/struct.DataFrame.html) in place.

See [`DataFrame::sort`](../prelude/struct.DataFrame.html#method.sort) for more instruction.

## [Source](../../src/polars_core/frame/mod.rs.html#1567)
#### pub fn [_to_metadata](#method._to_metadata)(&self) -> [DataFrame](../prelude/struct.DataFrame.html)

[Source](../../src/polars_core/frame/mod.rs.html#1567)

#### pub fn [_to_metadata](#method._to_metadata)(&self) -> [DataFrame](../prelude/struct.DataFrame.html)

Create a `DataFrame` that has fields for all the known runtime metadata for each column.

This dataframe does not necessarily have a specified schema and may be changed at any point. It is primarily used for debugging.

## [Source](../../src/polars_core/frame/mod.rs.html#1652-1656)
#### pub fn [sort](#method.sort)(
    &self,
    by: impl [IntoIterator](https://doc.rust-lang.org/nightly/core/iter/traits/collect/trait.IntoIterator.html)<Item = impl [AsRef](https://doc.rust-lang.org/nightly/core/convert/trait.AsRef.html)<[str](https://doc.rust-lang.org/nightly/std/primitive.str.html)>>,
    sort_options: [SortMultipleOptions](../prelude/struct.SortMultipleOptions.html),
) -> [Result](https://doc.rust-lang.org/nightly/core/result/enum.Result.html)<[DataFrame](../prelude/struct.DataFrame.html), [PolarsError](../prelude/enum.PolarsError.html)>

[Source](../../src/polars_core/frame/mod.rs.html#1652-1656)

#### pub fn [sort](#method.sort)(
    &self,
    by: impl [IntoIterator](https://doc.rust-lang.org/nightly/core/iter/traits/collect/trait.IntoIterator.html)<Item = impl [AsRef](https://doc.rust-lang.org/nightly/core/convert/trait.AsRef.html)<[str](https://doc.rust-lang.org/nightly/std/primitive.str.html)>>,
    sort_options: [SortMultipleOptions](../prelude/struct.SortMultipleOptions.html),
) -> [Result](https://doc.rust-lang.org/nightly/core/result/enum.Result.html)<[DataFrame](../prelude/struct.DataFrame.html), [PolarsError](../prelude/enum.PolarsError.html)>

Return a sorted clone of this [`DataFrame`](../prelude/struct.DataFrame.html).

In many cases the output chunks will be continuous in memory but this is not guaranteed

#####Example

Sort by a single column with default options:

```
fn sort_by_sepal_width(df: &DataFrame) -> PolarsResult<DataFrame> {
    df.sort(["sepal_width"], Default::default())
}
```
Sort by a single column with specific order:

```
fn sort_with_specific_order(df: &DataFrame, descending: bool) -> PolarsResult<DataFrame> {
    df.sort(
        ["sepal_width"],
        SortMultipleOptions::new()
            .with_order_descending(descending)
    )
}
```
Sort by multiple columns with specifying order for each column:

```
fn sort_by_multiple_columns_with_specific_order(df: &DataFrame) -> PolarsResult<DataFrame> {
    df.sort(
        ["sepal_width", "sepal_length"],
        SortMultipleOptions::new()
            .with_order_descending_multi([false, true])
    )
}
```
See [`SortMultipleOptions`](../prelude/struct.SortMultipleOptions.html) for more options.

Also see [`DataFrame::sort_in_place`](../prelude/struct.DataFrame.html#method.sort_in_place).

## [Source](../../src/polars_core/frame/mod.rs.html#1676)
#### pub fn [replace](#method.replace)(
    &mut self,
    column: &[str](https://doc.rust-lang.org/nightly/std/primitive.str.html),
    new_col: [Column](../prelude/enum.Column.html),
) -> [Result](https://doc.rust-lang.org/nightly/core/result/enum.Result.html)<&mut [DataFrame](../prelude/struct.DataFrame.html), [PolarsError](../prelude/enum.PolarsError.html)>

[Source](../../src/polars_core/frame/mod.rs.html#1676)

#### pub fn [replace](#method.replace)(
    &mut self,
    column: &[str](https://doc.rust-lang.org/nightly/std/primitive.str.html),
    new_col: [Column](../prelude/enum.Column.html),
) -> [Result](https://doc.rust-lang.org/nightly/core/result/enum.Result.html)<&mut [DataFrame](../prelude/struct.DataFrame.html), [PolarsError](../prelude/enum.PolarsError.html)>

Replace a column with a [`Column`](../prelude/enum.Column.html).

#####Example

```
let mut df: DataFrame = df!("Country" => ["United States", "China"],
                        "Area (km²)" => [9_833_520, 9_596_961])?;
let s: Column = Column::new("Country".into(), ["USA", "PRC"]);
assert!(df.replace("Nation", s.clone()).is_err());
assert!(df.replace("Country", s).is_ok());
```
## [Source](../../src/polars_core/frame/mod.rs.html#1694)
#### pub fn [replace_column](#method.replace_column)(
    &mut self,
    index: [usize](https://doc.rust-lang.org/nightly/std/primitive.usize.html),
    new_column: [Column](../prelude/enum.Column.html),
) -> [Result](https://doc.rust-lang.org/nightly/core/result/enum.Result.html)<&mut [DataFrame](../prelude/struct.DataFrame.html), [PolarsError](../prelude/enum.PolarsError.html)>

[Source](../../src/polars_core/frame/mod.rs.html#1694)

#### pub fn [replace_column](#method.replace_column)(
    &mut self,
    index: [usize](https://doc.rust-lang.org/nightly/std/primitive.usize.html),
    new_column: [Column](../prelude/enum.Column.html),
) -> [Result](https://doc.rust-lang.org/nightly/core/result/enum.Result.html)<&mut [DataFrame](../prelude/struct.DataFrame.html), [PolarsError](../prelude/enum.PolarsError.html)>

Replace column at index `idx` with a [`Series`](../prelude/struct.Series.html).

#####Example

```
# use polars_core::prelude::*;
let s0 = Series::new("foo".into(), ["ham", "spam", "egg"]);
let s1 = Series::new("ascii".into(), [70, 79, 79]);
let mut df = DataFrame::new_infer_height(vec![s0, s1])?;
// Add 32 to get lowercase ascii values
df.replace_column(1, df.select_at_idx(1).unwrap() + 32);
# Ok::<(), PolarsError>(())
```
## [Source](../../src/polars_core/frame/mod.rs.html#1754-1757)
#### pub fn [apply](#method.apply)<F, C>(
    &mut self,
    name: &[str](https://doc.rust-lang.org/nightly/std/primitive.str.html),
    f: F,
) -> [Result](https://doc.rust-lang.org/nightly/core/result/enum.Result.html)<&mut [DataFrame](../prelude/struct.DataFrame.html), [PolarsError](../prelude/enum.PolarsError.html)>

[Source](../../src/polars_core/frame/mod.rs.html#1754-1757)

#### pub fn [apply](#method.apply)<F, C>(
    &mut self,
    name: &[str](https://doc.rust-lang.org/nightly/std/primitive.str.html),
    f: F,
) -> [Result](https://doc.rust-lang.org/nightly/core/result/enum.Result.html)<&mut [DataFrame](../prelude/struct.DataFrame.html), [PolarsError](../prelude/enum.PolarsError.html)>

Apply a closure to a column. This is the recommended way to do in place modification.

#####Example

```
let s0 = Column::new("foo".into(), ["ham", "spam", "egg"]);
let s1 = Column::new("names".into(), ["Jean", "Claude", "van"]);
let mut df = DataFrame::new_infer_height(vec![s0, s1])?;
fn str_to_len(str_val: &Column) -> Column {
    str_val.str()
        .unwrap()
        .iter()
        .map(|opt_name: Option<&str>| {
            opt_name.map(|name: &str| name.len() as u32)
         })
        .collect::<UInt32Chunked>()
        .into_column()
}
// Replace the names column by the length of the names.
df.apply("names", str_to_len);
```
Results in:

```
+--------+-------+
| foo    |       |
| ---    | names |
| str    | u32   |
+========+=======+
| "ham"  | 4     |
+--------+-------+
| "spam" | 6     |
+--------+-------+
| "egg"  | 3     |
+--------+-------+
```
## [Source](../../src/polars_core/frame/mod.rs.html#1794-1797)
#### pub fn [apply_at_idx](#method.apply_at_idx)<F, C>(
    &mut self,
    idx: [usize](https://doc.rust-lang.org/nightly/std/primitive.usize.html),
    f: F,
) -> [Result](https://doc.rust-lang.org/nightly/core/result/enum.Result.html)<&mut [DataFrame](../prelude/struct.DataFrame.html), [PolarsError](../prelude/enum.PolarsError.html)>

[Source](../../src/polars_core/frame/mod.rs.html#1794-1797)

#### pub fn [apply_at_idx](#method.apply_at_idx)<F, C>(
    &mut self,
    idx: [usize](https://doc.rust-lang.org/nightly/std/primitive.usize.html),
    f: F,
) -> [Result](https://doc.rust-lang.org/nightly/core/result/enum.Result.html)<&mut [DataFrame](../prelude/struct.DataFrame.html), [PolarsError](../prelude/enum.PolarsError.html)>

Apply a closure to a column at index `idx`. This is the recommended way to do in place
modification.

#####Example

```
let s0 = Column::new("foo".into(), ["ham", "spam", "egg"]);
let s1 = Column::new("ascii".into(), [70, 79, 79]);
let mut df = DataFrame::new_infer_height(vec![s0, s1])?;
// Add 32 to get lowercase ascii values
df.apply_at_idx(1, |s| s + 32);
```
Results in:

```
+--------+-------+
| foo    | ascii |
| ---    | ---   |
| str    | i32   |
+========+=======+
| "ham"  | 102   |
+--------+-------+
| "spam" | 111   |
+--------+-------+
| "egg"  | 111   |
+--------+-------+
```
## [Source](../../src/polars_core/frame/mod.rs.html#1864-1867)
#### pub fn [try_apply_at_idx](#method.try_apply_at_idx)<F, C>(
    &mut self,
    idx: [usize](https://doc.rust-lang.org/nightly/std/primitive.usize.html),
    f: F,
) -> [Result](https://doc.rust-lang.org/nightly/core/result/enum.Result.html)<&mut [DataFrame](../prelude/struct.DataFrame.html), [PolarsError](../prelude/enum.PolarsError.html)>

[Source](../../src/polars_core/frame/mod.rs.html#1864-1867)

#### pub fn [try_apply_at_idx](#method.try_apply_at_idx)<F, C>(
    &mut self,
    idx: [usize](https://doc.rust-lang.org/nightly/std/primitive.usize.html),
    f: F,
) -> [Result](https://doc.rust-lang.org/nightly/core/result/enum.Result.html)<&mut [DataFrame](../prelude/struct.DataFrame.html), [PolarsError](../prelude/enum.PolarsError.html)>

Apply a closure that may fail to a column at index `idx`. This is the recommended way to do in place
modification.

#####Example

This is the idiomatic way to replace some values a column of a `DataFrame` given range of indexes.

```
let s0 = Column::new("foo".into(), ["ham", "spam", "egg", "bacon", "quack"]);
let s1 = Column::new("values".into(), [1, 2, 3, 4, 5]);
let mut df = DataFrame::new_infer_height(vec![s0, s1])?;
let idx = vec![0, 1, 4];
df.try_apply("foo", |c| {
    c.str()?
    .scatter_with(idx, |opt_val| opt_val.map(|string| format!("{}-is-modified", string)))
});
```
Results in:

```
+---------------------+--------+
| foo                 | values |
| ---                 | ---    |
| str                 | i32    |
+=====================+========+
| "ham-is-modified"   | 1      |
+---------------------+--------+
| "spam-is-modified"  | 2      |
+---------------------+--------+
| "egg"               | 3      |
+---------------------+--------+
| "bacon"             | 4      |
+---------------------+--------+
| "quack-is-modified" | 5      |
+---------------------+--------+
```
## [Source](../../src/polars_core/frame/mod.rs.html#1943-1946)
#### pub fn [try_apply](#method.try_apply)<F, C>(
    &mut self,
    column: &[str](https://doc.rust-lang.org/nightly/std/primitive.str.html),
    f: F,
) -> [Result](https://doc.rust-lang.org/nightly/core/result/enum.Result.html)<&mut [DataFrame](../prelude/struct.DataFrame.html), [PolarsError](../prelude/enum.PolarsError.html)>

[Source](../../src/polars_core/frame/mod.rs.html#1943-1946)

#### pub fn [try_apply](#method.try_apply)<F, C>(
    &mut self,
    column: &[str](https://doc.rust-lang.org/nightly/std/primitive.str.html),
    f: F,
) -> [Result](https://doc.rust-lang.org/nightly/core/result/enum.Result.html)<&mut [DataFrame](../prelude/struct.DataFrame.html), [PolarsError](../prelude/enum.PolarsError.html)>

Apply a closure that may fail to a column. This is the recommended way to do in place modification.

#####Example

This is the idiomatic way to replace some values a column of a `DataFrame` given a boolean mask.

```
let s0 = Column::new("foo".into(), ["ham", "spam", "egg", "bacon", "quack"]);
let s1 = Column::new("values".into(), [1, 2, 3, 4, 5]);
let mut df = DataFrame::new_infer_height(vec![s0, s1])?;
// create a mask
let values = df.column("values")?.as_materialized_series();
let mask = values.lt_eq(1)? | values.gt_eq(5_i32)?;
df.try_apply("foo", |c| {
    c.str()?
    .set(&mask, Some("not_within_bounds"))
});
```
Results in:

```
+---------------------+--------+
| foo                 | values |
| ---                 | ---    |
| str                 | i32    |
+=====================+========+
| "not_within_bounds" | 1      |
+---------------------+--------+
| "spam"              | 2      |
+---------------------+--------+
| "egg"               | 3      |
+---------------------+--------+
| "bacon"             | 4      |
+---------------------+--------+
| "not_within_bounds" | 5      |
+---------------------+--------+
```
## [Source](../../src/polars_core/frame/mod.rs.html#1982)
#### pub fn [slice](#method.slice)(&self, offset: [i64](https://doc.rust-lang.org/nightly/std/primitive.i64.html), length: [usize](https://doc.rust-lang.org/nightly/std/primitive.usize.html)) -> [DataFrame](../prelude/struct.DataFrame.html)

[Source](../../src/polars_core/frame/mod.rs.html#1982)

#### pub fn [slice](#method.slice)(&self, offset: [i64](https://doc.rust-lang.org/nightly/std/primitive.i64.html), length: [usize](https://doc.rust-lang.org/nightly/std/primitive.usize.html)) -> [DataFrame](../prelude/struct.DataFrame.html)

Slice the [`DataFrame`](../prelude/struct.DataFrame.html) along the rows.

#####Example

```
let df: DataFrame = df!("Fruit" => ["Apple", "Grape", "Grape", "Fig", "Fig"],
                        "Color" => ["Green", "Red", "White", "White", "Red"])?;
let sl: DataFrame = df.slice(2, 3);
assert_eq!(sl.shape(), (3, 2));
println!("{}", sl);
```
Output:

```
shape: (3, 2)
+-------+-------+
| Fruit | Color |
| ---   | ---   |
| str   | str   |
+=======+=======+
| Grape | White |
+-------+-------+
| Fig   | White |
+-------+-------+
| Fig   | Red   |
+-------+-------+
```
## [Source](../../src/polars_core/frame/mod.rs.html#2004)
#### pub fn [split_at](#method.split_at)(&self, offset: [i64](https://doc.rust-lang.org/nightly/std/primitive.i64.html)) -> ([DataFrame](../prelude/struct.DataFrame.html), [DataFrame](../prelude/struct.DataFrame.html))

[Source](../../src/polars_core/frame/mod.rs.html#2004)

#### pub fn [split_at](#method.split_at)(&self, offset: [i64](https://doc.rust-lang.org/nightly/std/primitive.i64.html)) -> ([DataFrame](../prelude/struct.DataFrame.html), [DataFrame](../prelude/struct.DataFrame.html))

Split [`DataFrame`](../prelude/struct.DataFrame.html) at the given `offset`.

[Source](../../src/polars_core/frame/mod.rs.html#2015)

#### pub fn [clear](#method.clear)(&self) -> [DataFrame](../prelude/struct.DataFrame.html)

[Source](../../src/polars_core/frame/mod.rs.html#2021)

#### pub fn [slice_par](#method.slice_par)(&self, offset: [i64](https://doc.rust-lang.org/nightly/std/primitive.i64.html), length: [usize](https://doc.rust-lang.org/nightly/std/primitive.usize.html)) -> [DataFrame](../prelude/struct.DataFrame.html)

[Source](../../src/polars_core/frame/mod.rs.html#2030)

#### pub fn [_slice_and_realloc](#method._slice_and_realloc)(&self, offset: [i64](https://doc.rust-lang.org/nightly/std/primitive.i64.html), length: [usize](https://doc.rust-lang.org/nightly/std/primitive.usize.html)) -> [DataFrame](../prelude/struct.DataFrame.html)

## [Source](../../src/polars_core/frame/mod.rs.html#2077)
#### pub fn [head](#method.head)(&self, length: [Option](https://doc.rust-lang.org/nightly/core/option/enum.Option.html)<[usize](https://doc.rust-lang.org/nightly/std/primitive.usize.html)>) -> [DataFrame](../prelude/struct.DataFrame.html)

[Source](../../src/polars_core/frame/mod.rs.html#2077)

#### pub fn [head](#method.head)(&self, length: [Option](https://doc.rust-lang.org/nightly/core/option/enum.Option.html)<[usize](https://doc.rust-lang.org/nightly/std/primitive.usize.html)>) -> [DataFrame](../prelude/struct.DataFrame.html)

Get the head of the [`DataFrame`](../prelude/struct.DataFrame.html).

#####Example

```
let countries: DataFrame =
    df!("Rank by GDP (2021)" => [1, 2, 3, 4, 5],
        "Continent" => ["North America", "Asia", "Asia", "Europe", "Europe"],
        "Country" => ["United States", "China", "Japan", "Germany", "United Kingdom"],
        "Capital" => ["Washington", "Beijing", "Tokyo", "Berlin", "London"])?;
assert_eq!(countries.shape(), (5, 4));
println!("{}", countries.head(Some(3)));
```
Output:

```
shape: (3, 4)
+--------------------+---------------+---------------+------------+
| Rank by GDP (2021) | Continent     | Country       | Capital    |
| ---                | ---           | ---           | ---        |
| i32                | str           | str           | str        |
+====================+===============+===============+============+
| 1                  | North America | United States | Washington |
+--------------------+---------------+---------------+------------+
| 2                  | Asia          | China         | Beijing    |
+--------------------+---------------+---------------+------------+
| 3                  | Asia          | Japan         | Tokyo      |
+--------------------+---------------+---------------+------------+
```
## [Source](../../src/polars_core/frame/mod.rs.html#2115)
#### pub fn [tail](#method.tail)(&self, length: [Option](https://doc.rust-lang.org/nightly/core/option/enum.Option.html)<[usize](https://doc.rust-lang.org/nightly/std/primitive.usize.html)>) -> [DataFrame](../prelude/struct.DataFrame.html)

[Source](../../src/polars_core/frame/mod.rs.html#2115)

#### pub fn [tail](#method.tail)(&self, length: [Option](https://doc.rust-lang.org/nightly/core/option/enum.Option.html)<[usize](https://doc.rust-lang.org/nightly/std/primitive.usize.html)>) -> [DataFrame](../prelude/struct.DataFrame.html)

Get the tail of the [`DataFrame`](../prelude/struct.DataFrame.html).

#####Example

```
let countries: DataFrame =
    df!("Rank (2021)" => [105, 106, 107, 108, 109],
        "Apple Price (€/kg)" => [0.75, 0.70, 0.70, 0.65, 0.52],
        "Country" => ["Kosovo", "Moldova", "North Macedonia", "Syria", "Turkey"])?;
assert_eq!(countries.shape(), (5, 3));
println!("{}", countries.tail(Some(2)));
```
Output:

```
shape: (2, 3)
+-------------+--------------------+---------+
| Rank (2021) | Apple Price (€/kg) | Country |
| ---         | ---                | ---     |
| i32         | f64                | str     |
+=============+====================+=========+
| 108         | 0.65               | Syria   |
+-------------+--------------------+---------+
| 109         | 0.52               | Turkey  |
+-------------+--------------------+---------+
```
## [Source](../../src/polars_core/frame/mod.rs.html#2131-2135)
#### pub fn [iter_chunks](#method.iter_chunks)(
    &self,
    compat_level: [CompatLevel](../prelude/struct.CompatLevel.html),
    parallel: [bool](https://doc.rust-lang.org/nightly/std/primitive.bool.html),
) -> impl [Iterator](https://doc.rust-lang.org/nightly/core/iter/traits/iterator/trait.Iterator.html)<Item = RecordBatchT<[Box](https://doc.rust-lang.org/nightly/alloc/boxed/struct.Box.html)<dyn Array>>>

[Source](../../src/polars_core/frame/mod.rs.html#2131-2135)

#### pub fn [iter_chunks](#method.iter_chunks)(
    &self,
    compat_level: [CompatLevel](../prelude/struct.CompatLevel.html),
    parallel: [bool](https://doc.rust-lang.org/nightly/std/primitive.bool.html),
) -> impl [Iterator](https://doc.rust-lang.org/nightly/core/iter/traits/iterator/trait.Iterator.html)<Item = RecordBatchT<[Box](https://doc.rust-lang.org/nightly/alloc/boxed/struct.Box.html)<dyn Array>>>

Iterator over the rows in this [`DataFrame`](../prelude/struct.DataFrame.html) as Arrow RecordBatches.

#####Panics

Panics if the [`DataFrame`](../prelude/struct.DataFrame.html) that is passed is not rechunked.

This responsibility is left to the caller as we don’t want to take mutable references here, but we also don’t want to rechunk here, as this operation is costly and would benefit the caller as well.

## [Source](../../src/polars_core/frame/mod.rs.html#2177)
#### pub fn [iter_chunks_physical](#method.iter_chunks_physical)(
    &self,
) -> impl [Iterator](https://doc.rust-lang.org/nightly/core/iter/traits/iterator/trait.Iterator.html)<Item = RecordBatchT<[Box](https://doc.rust-lang.org/nightly/alloc/boxed/struct.Box.html)<dyn Array>>>

[Source](../../src/polars_core/frame/mod.rs.html#2177)

#### pub fn [iter_chunks_physical](#method.iter_chunks_physical)(
    &self,
) -> impl [Iterator](https://doc.rust-lang.org/nightly/core/iter/traits/iterator/trait.Iterator.html)<Item = RecordBatchT<[Box](https://doc.rust-lang.org/nightly/alloc/boxed/struct.Box.html)<dyn Array>>>

Iterator over the rows in this [`DataFrame`](../prelude/struct.DataFrame.html) as Arrow RecordBatches as physical values.

#####Panics

Panics if the [`DataFrame`](../prelude/struct.DataFrame.html) that is passed is not rechunked.

This responsibility is left to the caller as we don’t want to take mutable references here, but we also don’t want to rechunk here, as this operation is costly and would benefit the caller as well.

## [Source](../../src/polars_core/frame/mod.rs.html#2210)
#### pub fn [shift](#method.shift)(&self, periods: [i64](https://doc.rust-lang.org/nightly/std/primitive.i64.html)) -> [DataFrame](../prelude/struct.DataFrame.html)

[Source](../../src/polars_core/frame/mod.rs.html#2210)

#### pub fn [shift](#method.shift)(&self, periods: [i64](https://doc.rust-lang.org/nightly/std/primitive.i64.html)) -> [DataFrame](../prelude/struct.DataFrame.html)

Shift the values by a given period and fill the parts that will be empty due to this operation
with `Nones`.

See the method on [Series](../prelude/trait.SeriesTrait.html#tymethod.shift) for more info on the `shift` operation.

## [Source](../../src/polars_core/frame/mod.rs.html#2223)
#### pub fn [fill_null](#method.fill_null)(
    &self,
    strategy: [FillNullStrategy](../prelude/enum.FillNullStrategy.html),
) -> [Result](https://doc.rust-lang.org/nightly/core/result/enum.Result.html)<[DataFrame](../prelude/struct.DataFrame.html), [PolarsError](../prelude/enum.PolarsError.html)>

[Source](../../src/polars_core/frame/mod.rs.html#2223)

#### pub fn [fill_null](#method.fill_null)(
    &self,
    strategy: [FillNullStrategy](../prelude/enum.FillNullStrategy.html),
) -> [Result](https://doc.rust-lang.org/nightly/core/result/enum.Result.html)<[DataFrame](../prelude/struct.DataFrame.html), [PolarsError](../prelude/enum.PolarsError.html)>

Replace None values with one of the following strategies:

- Forward fill (replace None with the previous value)
- Backward fill (replace None with the next value)
- Mean fill (replace None with the mean of the whole array)
- Min fill (replace None with the minimum of the whole array)
- Max fill (replace None with the maximum of the whole array)

See the method on [Series](../prelude/struct.Series.html#method.fill_null) for more info on the `fill_null` operation.

## [Source](../../src/polars_core/frame/mod.rs.html#2230-2232)
#### pub fn [pipe](#method.pipe)<F, B>(self, f: F) -> [Result](https://doc.rust-lang.org/nightly/core/result/enum.Result.html)<B, [PolarsError](../prelude/enum.PolarsError.html)>

[Source](../../src/polars_core/frame/mod.rs.html#2230-2232)

#### pub fn [pipe](#method.pipe)<F, B>(self, f: F) -> [Result](https://doc.rust-lang.org/nightly/core/result/enum.Result.html)<B, [PolarsError](../prelude/enum.PolarsError.html)>

Pipe different functions/ closure operations that work on a DataFrame together.

## [Source](../../src/polars_core/frame/mod.rs.html#2238-2240)
#### pub fn [pipe_mut](#method.pipe_mut)<F, B>(&mut self, f: F) -> [Result](https://doc.rust-lang.org/nightly/core/result/enum.Result.html)<B, [PolarsError](../prelude/enum.PolarsError.html)>

[Source](../../src/polars_core/frame/mod.rs.html#2238-2240)

#### pub fn [pipe_mut](#method.pipe_mut)<F, B>(&mut self, f: F) -> [Result](https://doc.rust-lang.org/nightly/core/result/enum.Result.html)<B, [PolarsError](../prelude/enum.PolarsError.html)>

Pipe different functions/ closure operations that work on a DataFrame together.

## [Source](../../src/polars_core/frame/mod.rs.html#2246-2248)
#### pub fn [pipe_with_args](#method.pipe_with_args)<F, B, Args>(
    self,
    f: F,
    args: Args,
) -> [Result](https://doc.rust-lang.org/nightly/core/result/enum.Result.html)<B, [PolarsError](../prelude/enum.PolarsError.html)>

[Source](../../src/polars_core/frame/mod.rs.html#2246-2248)

#### pub fn [pipe_with_args](#method.pipe_with_args)<F, B, Args>(
    self,
    f: F,
    args: Args,
) -> [Result](https://doc.rust-lang.org/nightly/core/result/enum.Result.html)<B, [PolarsError](../prelude/enum.PolarsError.html)>

Pipe different functions/ closure operations that work on a DataFrame together.

## [Source](../../src/polars_core/frame/mod.rs.html#2286-2291)
#### pub fn [unique_stable](#method.unique_stable)(
    &self,
    subset: [Option](https://doc.rust-lang.org/nightly/core/option/enum.Option.html)<&[[String](https://doc.rust-lang.org/nightly/alloc/string/struct.String.html)]>,
    keep: [UniqueKeepStrategy](../prelude/enum.UniqueKeepStrategy.html),
    slice: [Option](https://doc.rust-lang.org/nightly/core/option/enum.Option.html)<([i64](https://doc.rust-lang.org/nightly/std/primitive.i64.html), [usize](https://doc.rust-lang.org/nightly/std/primitive.usize.html))>,
) -> [Result](https://doc.rust-lang.org/nightly/core/result/enum.Result.html)<[DataFrame](../prelude/struct.DataFrame.html), [PolarsError](../prelude/enum.PolarsError.html)>

**crate feature `algorithm_group_by`** only.

[Source](../../src/polars_core/frame/mod.rs.html#2286-2291)

#### pub fn [unique_stable](#method.unique_stable)(
    &self,
    subset: [Option](https://doc.rust-lang.org/nightly/core/option/enum.Option.html)<&[[String](https://doc.rust-lang.org/nightly/alloc/string/struct.String.html)]>,
    keep: [UniqueKeepStrategy](../prelude/enum.UniqueKeepStrategy.html),
    slice: [Option](https://doc.rust-lang.org/nightly/core/option/enum.Option.html)<([i64](https://doc.rust-lang.org/nightly/std/primitive.i64.html), [usize](https://doc.rust-lang.org/nightly/std/primitive.usize.html))>,
) -> [Result](https://doc.rust-lang.org/nightly/core/result/enum.Result.html)<[DataFrame](../prelude/struct.DataFrame.html), [PolarsError](../prelude/enum.PolarsError.html)>

**crate feature**only.

`algorithm_group_by`
Drop duplicate rows from a [`DataFrame`](../prelude/struct.DataFrame.html).
*This fails when there is a column of type List in DataFrame*

Stable means that the order is maintained. This has a higher cost than an unstable distinct.

#####Example

```
let df = df! {
              "flt" => [1., 1., 2., 2., 3., 3.],
              "int" => [1, 1, 2, 2, 3, 3, ],
              "str" => ["a", "a", "b", "b", "c", "c"]
          }?;
println!("{}", df.unique_stable(None, UniqueKeepStrategy::First, None)?);
```
Returns

```
+-----+-----+-----+
| flt | int | str |
| --- | --- | --- |
| f64 | i32 | str |
+=====+=====+=====+
| 1   | 1   | "a" |
+-----+-----+-----+
| 2   | 2   | "b" |
+-----+-----+-----+
| 3   | 3   | "c" |
+-----+-----+-----+
```
## [Source](../../src/polars_core/frame/mod.rs.html#2302-2307)
#### pub fn [unique](#method.unique)<I, S>(
    &self,
    subset: [Option](https://doc.rust-lang.org/nightly/core/option/enum.Option.html)<&[[String](https://doc.rust-lang.org/nightly/alloc/string/struct.String.html)]>,
    keep: [UniqueKeepStrategy](../prelude/enum.UniqueKeepStrategy.html),
    slice: [Option](https://doc.rust-lang.org/nightly/core/option/enum.Option.html)<([i64](https://doc.rust-lang.org/nightly/std/primitive.i64.html), [usize](https://doc.rust-lang.org/nightly/std/primitive.usize.html))>,
) -> [Result](https://doc.rust-lang.org/nightly/core/result/enum.Result.html)<[DataFrame](../prelude/struct.DataFrame.html), [PolarsError](../prelude/enum.PolarsError.html)>

**crate feature `algorithm_group_by`** only.

[Source](../../src/polars_core/frame/mod.rs.html#2302-2307)

#### pub fn [unique](#method.unique)<I, S>(
    &self,
    subset: [Option](https://doc.rust-lang.org/nightly/core/option/enum.Option.html)<&[[String](https://doc.rust-lang.org/nightly/alloc/string/struct.String.html)]>,
    keep: [UniqueKeepStrategy](../prelude/enum.UniqueKeepStrategy.html),
    slice: [Option](https://doc.rust-lang.org/nightly/core/option/enum.Option.html)<([i64](https://doc.rust-lang.org/nightly/std/primitive.i64.html), [usize](https://doc.rust-lang.org/nightly/std/primitive.usize.html))>,
) -> [Result](https://doc.rust-lang.org/nightly/core/result/enum.Result.html)<[DataFrame](../prelude/struct.DataFrame.html), [PolarsError](../prelude/enum.PolarsError.html)>

**crate feature**only.

`algorithm_group_by`
Unstable distinct. See [`DataFrame::unique_stable`](../prelude/struct.DataFrame.html#method.unique_stable).

[Source](../../src/polars_core/frame/mod.rs.html#2317-2323)

#### pub fn [unique_impl](#method.unique_impl)(
    &self,
    maintain_order: [bool](https://doc.rust-lang.org/nightly/std/primitive.bool.html),
    subset: [Option](https://doc.rust-lang.org/nightly/core/option/enum.Option.html)<[Vec](https://doc.rust-lang.org/nightly/alloc/vec/struct.Vec.html)<[PlSmallStr](../prelude/struct.PlSmallStr.html)>>,
    keep: [UniqueKeepStrategy](../prelude/enum.UniqueKeepStrategy.html),
    slice: [Option](https://doc.rust-lang.org/nightly/core/option/enum.Option.html)<([i64](https://doc.rust-lang.org/nightly/std/primitive.i64.html), [usize](https://doc.rust-lang.org/nightly/std/primitive.usize.html))>,
) -> [Result](https://doc.rust-lang.org/nightly/core/result/enum.Result.html)<[DataFrame](../prelude/struct.DataFrame.html), [PolarsError](../prelude/enum.PolarsError.html)>

**crate feature**only.

`algorithm_group_by`
## [Source](../../src/polars_core/frame/mod.rs.html#2408)
#### pub fn [is_unique](#method.is_unique)(&self) -> [Result](https://doc.rust-lang.org/nightly/core/result/enum.Result.html)<[ChunkedArray](../prelude/struct.ChunkedArray.html)<[BooleanType](../prelude/struct.BooleanType.html)>, [PolarsError](../prelude/enum.PolarsError.html)>

**crate feature `algorithm_group_by`** only.

[Source](../../src/polars_core/frame/mod.rs.html#2408)

#### pub fn [is_unique](#method.is_unique)(&self) -> [Result](https://doc.rust-lang.org/nightly/core/result/enum.Result.html)<[ChunkedArray](../prelude/struct.ChunkedArray.html)<[BooleanType](../prelude/struct.BooleanType.html)>, [PolarsError](../prelude/enum.PolarsError.html)>

**crate feature**only.

`algorithm_group_by`
## [Source](../../src/polars_core/frame/mod.rs.html#2433)
#### pub fn [is_duplicated](#method.is_duplicated)(&self) -> [Result](https://doc.rust-lang.org/nightly/core/result/enum.Result.html)<[ChunkedArray](../prelude/struct.ChunkedArray.html)<[BooleanType](../prelude/struct.BooleanType.html)>, [PolarsError](../prelude/enum.PolarsError.html)>

**crate feature `algorithm_group_by`** only.

[Source](../../src/polars_core/frame/mod.rs.html#2433)

#### pub fn [is_duplicated](#method.is_duplicated)(&self) -> [Result](https://doc.rust-lang.org/nightly/core/result/enum.Result.html)<[ChunkedArray](../prelude/struct.ChunkedArray.html)<[BooleanType](../prelude/struct.BooleanType.html)>, [PolarsError](../prelude/enum.PolarsError.html)>

**crate feature**only.

`algorithm_group_by`
## [Source](../../src/polars_core/frame/mod.rs.html#2446)
#### pub fn [null_count](#method.null_count)(&self) -> [DataFrame](../prelude/struct.DataFrame.html)

[Source](../../src/polars_core/frame/mod.rs.html#2446)

#### pub fn [null_count](#method.null_count)(&self) -> [DataFrame](../prelude/struct.DataFrame.html)

Create a new [`DataFrame`](../prelude/struct.DataFrame.html) that shows the null counts per column.

## [Source](../../src/polars_core/frame/mod.rs.html#2470)
#### pub fn [get_supertype](#method.get_supertype)(&self) -> [Option](https://doc.rust-lang.org/nightly/core/option/enum.Option.html)<[Result](https://doc.rust-lang.org/nightly/core/result/enum.Result.html)<[DataType](../prelude/enum.DataType.html), [PolarsError](../prelude/enum.PolarsError.html)>>

[Source](../../src/polars_core/frame/mod.rs.html#2470)

#### pub fn [get_supertype](#method.get_supertype)(&self) -> [Option](https://doc.rust-lang.org/nightly/core/option/enum.Option.html)<[Result](https://doc.rust-lang.org/nightly/core/result/enum.Result.html)<[DataType](../prelude/enum.DataType.html), [PolarsError](../prelude/enum.PolarsError.html)>>

Get the supertype of the columns in this DataFrame

## [Source](../../src/polars_core/frame/mod.rs.html#2594-2597)
#### pub fn [partition_by](#method.partition_by)<I, S>(
    &self,
    cols: I,
    include_key: [bool](https://doc.rust-lang.org/nightly/std/primitive.bool.html),
) -> [Result](https://doc.rust-lang.org/nightly/core/result/enum.Result.html)<[Vec](https://doc.rust-lang.org/nightly/alloc/vec/struct.Vec.html)<[DataFrame](../prelude/struct.DataFrame.html)>, [PolarsError](../prelude/enum.PolarsError.html)>

**crate feature `partition_by`** only.

[Source](../../src/polars_core/frame/mod.rs.html#2594-2597)

#### pub fn [partition_by](#method.partition_by)<I, S>(
    &self,
    cols: I,
    include_key: [bool](https://doc.rust-lang.org/nightly/std/primitive.bool.html),
) -> [Result](https://doc.rust-lang.org/nightly/core/result/enum.Result.html)<[Vec](https://doc.rust-lang.org/nightly/alloc/vec/struct.Vec.html)<[DataFrame](../prelude/struct.DataFrame.html)>, [PolarsError](../prelude/enum.PolarsError.html)>

**crate feature**only.

`partition_by`
Split into multiple DataFrames partitioned by groups

## [Source](../../src/polars_core/frame/mod.rs.html#2606-2613)
#### pub fn [partition_by_stable](#method.partition_by_stable)<I, S>(
    &self,
    cols: I,
    include_key: [bool](https://doc.rust-lang.org/nightly/std/primitive.bool.html),
) -> [Result](https://doc.rust-lang.org/nightly/core/result/enum.Result.html)<[Vec](https://doc.rust-lang.org/nightly/alloc/vec/struct.Vec.html)<[DataFrame](../prelude/struct.DataFrame.html)>, [PolarsError](../prelude/enum.PolarsError.html)>

**crate feature `partition_by`** only.

[Source](../../src/polars_core/frame/mod.rs.html#2606-2613)

#### pub fn [partition_by_stable](#method.partition_by_stable)<I, S>(
    &self,
    cols: I,
    include_key: [bool](https://doc.rust-lang.org/nightly/std/primitive.bool.html),
) -> [Result](https://doc.rust-lang.org/nightly/core/result/enum.Result.html)<[Vec](https://doc.rust-lang.org/nightly/alloc/vec/struct.Vec.html)<[DataFrame](../prelude/struct.DataFrame.html)>, [PolarsError](../prelude/enum.PolarsError.html)>

**crate feature**only.

`partition_by`
Split into multiple DataFrames partitioned by groups Order of the groups are maintained.

## [Source](../../src/polars_core/frame/mod.rs.html#2622-2626)
#### pub fn [unnest](#method.unnest)(
    &self,
    cols: impl [IntoIterator](https://doc.rust-lang.org/nightly/core/iter/traits/collect/trait.IntoIterator.html)<Item = impl [Into](https://doc.rust-lang.org/nightly/core/convert/trait.Into.html)<[PlSmallStr](../prelude/struct.PlSmallStr.html)>>,
    separator: [Option](https://doc.rust-lang.org/nightly/core/option/enum.Option.html)<&[str](https://doc.rust-lang.org/nightly/std/primitive.str.html)>,
) -> [Result](https://doc.rust-lang.org/nightly/core/result/enum.Result.html)<[DataFrame](../prelude/struct.DataFrame.html), [PolarsError](../prelude/enum.PolarsError.html)>

**crate feature `dtype-struct`** only.

[Source](../../src/polars_core/frame/mod.rs.html#2622-2626)

#### pub fn [unnest](#method.unnest)(
    &self,
    cols: impl [IntoIterator](https://doc.rust-lang.org/nightly/core/iter/traits/collect/trait.IntoIterator.html)<Item = impl [Into](https://doc.rust-lang.org/nightly/core/convert/trait.Into.html)<[PlSmallStr](../prelude/struct.PlSmallStr.html)>>,
    separator: [Option](https://doc.rust-lang.org/nightly/core/option/enum.Option.html)<&[str](https://doc.rust-lang.org/nightly/std/primitive.str.html)>,
) -> [Result](https://doc.rust-lang.org/nightly/core/result/enum.Result.html)<[DataFrame](../prelude/struct.DataFrame.html), [PolarsError](../prelude/enum.PolarsError.html)>

**crate feature**only.

`dtype-struct`
Unnest the given `Struct` columns. This means that the fields of the `Struct` type will be
inserted as columns.

[Source](../../src/polars_core/frame/mod.rs.html#2671)

#### pub fn [append_record_batch](#method.append_record_batch)(
    &mut self,
    rb: RecordBatchT<[Box](https://doc.rust-lang.org/nightly/alloc/boxed/struct.Box.html)<dyn Array>>,
) -> [Result](https://doc.rust-lang.org/nightly/core/result/enum.Result.html)<[()](https://doc.rust-lang.org/nightly/std/primitive.unit.html), [PolarsError](../prelude/enum.PolarsError.html)>

## [Source](../../src/polars_core/serde/df.rs.html#23)
### impl [DataFrame](../prelude/struct.DataFrame.html)

[Source](../../src/polars_core/serde/df.rs.html#23)

### impl [DataFrame](../prelude/struct.DataFrame.html)

[Source](../../src/polars_core/serde/df.rs.html#24)

#### pub fn [serialize_into_writer](#method.serialize_into_writer)(
    &mut self,
    writer: &mut dyn [Write](https://doc.rust-lang.org/nightly/core/io/write/trait.Write.html),
) -> [Result](https://doc.rust-lang.org/nightly/core/result/enum.Result.html)<[()](https://doc.rust-lang.org/nightly/std/primitive.unit.html), [PolarsError](../prelude/enum.PolarsError.html)>

[Source](../../src/polars_core/serde/df.rs.html#71)

#### pub fn [serialize_to_bytes](#method.serialize_to_bytes)(&mut self) -> [Result](https://doc.rust-lang.org/nightly/core/result/enum.Result.html)<[Vec](https://doc.rust-lang.org/nightly/alloc/vec/struct.Vec.html)<[u8](https://doc.rust-lang.org/nightly/std/primitive.u8.html)>, [PolarsError](../prelude/enum.PolarsError.html)>

[Source](../../src/polars_core/serde/df.rs.html#78)

#### pub fn [deserialize_from_reader](#method.deserialize_from_reader)<T>(
    reader: [&mut T](https://doc.rust-lang.org/nightly/std/primitive.reference.html),
) -> [Result](https://doc.rust-lang.org/nightly/core/result/enum.Result.html)<[DataFrame](../prelude/struct.DataFrame.html), [PolarsError](../prelude/enum.PolarsError.html)>

## [Source](../../src/polars_core/testing.rs.html#44)
### impl [DataFrame](../prelude/struct.DataFrame.html)

[Source](../../src/polars_core/testing.rs.html#44)

### impl [DataFrame](../prelude/struct.DataFrame.html)

## [Source](../../src/polars_core/testing.rs.html#46)
#### pub fn [schema_equal](#method.schema_equal)(&self, other: &[DataFrame](../prelude/struct.DataFrame.html)) -> [Result](https://doc.rust-lang.org/nightly/core/result/enum.Result.html)<[()](https://doc.rust-lang.org/nightly/std/primitive.unit.html), [PolarsError](../prelude/enum.PolarsError.html)>

[Source](../../src/polars_core/testing.rs.html#46)

#### pub fn [schema_equal](#method.schema_equal)(&self, other: &[DataFrame](../prelude/struct.DataFrame.html)) -> [Result](https://doc.rust-lang.org/nightly/core/result/enum.Result.html)<[()](https://doc.rust-lang.org/nightly/std/primitive.unit.html), [PolarsError](../prelude/enum.PolarsError.html)>

Check if [`DataFrame`](../prelude/struct.DataFrame.html)’ schemas are equal.

## [Source](../../src/polars_core/testing.rs.html#76)
#### pub fn [equals](#method.equals)(&self, other: &[DataFrame](../prelude/struct.DataFrame.html)) -> [bool](https://doc.rust-lang.org/nightly/std/primitive.bool.html)

[Source](../../src/polars_core/testing.rs.html#76)

#### pub fn [equals](#method.equals)(&self, other: &[DataFrame](../prelude/struct.DataFrame.html)) -> [bool](https://doc.rust-lang.org/nightly/std/primitive.bool.html)

Check if [`DataFrame`](../prelude/struct.DataFrame.html)s are equal. Note that `None == None` evaluates to `false`

#####Example

```
let df1: DataFrame = df!("Atomic number" => &[1, 51, 300],
                        "Element" => &[Some("Hydrogen"), Some("Antimony"), None])?;
let df2: DataFrame = df!("Atomic number" => &[1, 51, 300],
                        "Element" => &[Some("Hydrogen"), Some("Antimony"), None])?;
assert!(!df1.equals(&df2));
```
## [Source](../../src/polars_core/testing.rs.html#102)
#### pub fn [equals_missing](#method.equals_missing)(&self, other: &[DataFrame](../prelude/struct.DataFrame.html)) -> [bool](https://doc.rust-lang.org/nightly/std/primitive.bool.html)

[Source](../../src/polars_core/testing.rs.html#102)

#### pub fn [equals_missing](#method.equals_missing)(&self, other: &[DataFrame](../prelude/struct.DataFrame.html)) -> [bool](https://doc.rust-lang.org/nightly/std/primitive.bool.html)

Check if all values in [`DataFrame`](../prelude/struct.DataFrame.html)s are equal where `None == None` evaluates to `true`.

#####Example

```
let df1: DataFrame = df!("Atomic number" => &[1, 51, 300],
                        "Element" => &[Some("Hydrogen"), Some("Antimony"), None])?;
let df2: DataFrame = df!("Atomic number" => &[1, 51, 300],
                        "Element" => &[Some("Hydrogen"), Some("Antimony"), None])?;
assert!(df1.equals_missing(&df2));
```
## Trait Implementations

[Source](../../src/polars_ops/frame/join/asof/mod.rs.html#396)

### impl [AsofJoin](../prelude/trait.AsofJoin.html) for [DataFrame](../prelude/struct.DataFrame.html)

## [Source](../../src/polars_ops/frame/join/asof/groups.rs.html#646)
### impl [AsofJoinBy](../prelude/trait.AsofJoinBy.html) for [DataFrame](../prelude/struct.DataFrame.html)

[Source](../../src/polars_ops/frame/join/asof/groups.rs.html#646)

### impl [AsofJoinBy](../prelude/trait.AsofJoinBy.html) for [DataFrame](../prelude/struct.DataFrame.html)

## [Source](../../src/polars_ops/frame/join/asof/groups.rs.html#608-622)
#### fn [join_asof_by](../prelude/trait.AsofJoinBy.html#method.join_asof_by)<I, S>(
    &self,
    other: &[DataFrame](../prelude/struct.DataFrame.html),
    left_on: &[str](https://doc.rust-lang.org/nightly/std/primitive.str.html),
    right_on: &[str](https://doc.rust-lang.org/nightly/std/primitive.str.html),
    left_by: I,
    right_by: I,
    strategy: [AsofStrategy](../prelude/enum.AsofStrategy.html),
    tolerance: [Option](https://doc.rust-lang.org/nightly/core/option/enum.Option.html)<[AnyValue](../prelude/enum.AnyValue.html)<'static>>,
    allow_eq: [bool](https://doc.rust-lang.org/nightly/std/primitive.bool.html),
    check_sortedness: [bool](https://doc.rust-lang.org/nightly/std/primitive.bool.html),
) -> [Result](https://doc.rust-lang.org/nightly/core/result/enum.Result.html)<[DataFrame](../prelude/struct.DataFrame.html), [PolarsError](../prelude/enum.PolarsError.html)>

[Source](../../src/polars_ops/frame/join/asof/groups.rs.html#608-622)

#### fn [join_asof_by](../prelude/trait.AsofJoinBy.html#method.join_asof_by)<I, S>(
    &self,
    other: &[DataFrame](../prelude/struct.DataFrame.html),
    left_on: &[str](https://doc.rust-lang.org/nightly/std/primitive.str.html),
    right_on: &[str](https://doc.rust-lang.org/nightly/std/primitive.str.html),
    left_by: I,
    right_by: I,
    strategy: [AsofStrategy](../prelude/enum.AsofStrategy.html),
    tolerance: [Option](https://doc.rust-lang.org/nightly/core/option/enum.Option.html)<[AnyValue](../prelude/enum.AnyValue.html)<'static>>,
    allow_eq: [bool](https://doc.rust-lang.org/nightly/std/primitive.bool.html),
    check_sortedness: [bool](https://doc.rust-lang.org/nightly/std/primitive.bool.html),
) -> [Result](https://doc.rust-lang.org/nightly/core/result/enum.Result.html)<[DataFrame](../prelude/struct.DataFrame.html), [PolarsError](../prelude/enum.PolarsError.html)>

`by`.
## [Source](../../src/polars_core/utils/mod.rs.html#124)
### impl [Container](../../polars_core/utils/trait.Container.html) for [DataFrame](../prelude/struct.DataFrame.html)

[Source](../../src/polars_core/utils/mod.rs.html#124)

### impl [Container](../../polars_core/utils/trait.Container.html) for [DataFrame](../prelude/struct.DataFrame.html)

[Source](../../src/polars_core/utils/mod.rs.html#125)

#### fn [slice](../../polars_core/utils/trait.Container.html#tymethod.slice)(&self, offset: [i64](https://doc.rust-lang.org/nightly/std/primitive.i64.html), len: [usize](https://doc.rust-lang.org/nightly/std/primitive.usize.html)) -> [DataFrame](../prelude/struct.DataFrame.html)

[Source](../../src/polars_core/utils/mod.rs.html#129)

#### fn [split_at](../../polars_core/utils/trait.Container.html#tymethod.split_at)(&self, offset: [i64](https://doc.rust-lang.org/nightly/std/primitive.i64.html)) -> ([DataFrame](../prelude/struct.DataFrame.html), [DataFrame](../prelude/struct.DataFrame.html))

[Source](../../src/polars_core/utils/mod.rs.html#133)

#### fn [len](../../polars_core/utils/trait.Container.html#tymethod.len)(&self) -> [usize](https://doc.rust-lang.org/nightly/std/primitive.usize.html)

[Source](../../src/polars_core/utils/mod.rs.html#137)

#### fn [iter_chunks](../../polars_core/utils/trait.Container.html#tymethod.iter_chunks)(&self) -> impl [Iterator](https://doc.rust-lang.org/nightly/core/iter/traits/iterator/trait.Iterator.html)<Item = [DataFrame](../prelude/struct.DataFrame.html)>

[Source](../../src/polars_core/utils/mod.rs.html#141)

#### fn [should_rechunk](../../polars_core/utils/trait.Container.html#tymethod.should_rechunk)(&self) -> [bool](https://doc.rust-lang.org/nightly/std/primitive.bool.html)

[Source](../../src/polars_core/utils/mod.rs.html#145)

#### fn [n_chunks](../../polars_core/utils/trait.Container.html#tymethod.n_chunks)(&self) -> [usize](https://doc.rust-lang.org/nightly/std/primitive.usize.html)

[Source](../../src/polars_core/utils/mod.rs.html#149)

#### fn [chunk_lengths](../../polars_core/utils/trait.Container.html#tymethod.chunk_lengths)(&self) -> impl [Iterator](https://doc.rust-lang.org/nightly/core/iter/traits/iterator/trait.Iterator.html)<Item = [usize](https://doc.rust-lang.org/nightly/std/primitive.usize.html)>

## [Source](../../src/polars_ops/frame/join/cross_join.rs.html#60)
### impl [CrossJoin](../prelude/trait.CrossJoin.html) for [DataFrame](../prelude/struct.DataFrame.html)

[Source](../../src/polars_ops/frame/join/cross_join.rs.html#60)

### impl [CrossJoin](../prelude/trait.CrossJoin.html) for [DataFrame](../prelude/struct.DataFrame.html)

## [Source](../../src/polars_ops/frame/join/cross_join.rs.html#47-53)
#### fn [cross_join](../prelude/trait.CrossJoin.html#method.cross_join)(
    &self,
    other: &[DataFrame](../prelude/struct.DataFrame.html),
    suffix: [Option](https://doc.rust-lang.org/nightly/core/option/enum.Option.html)<[PlSmallStr](../prelude/struct.PlSmallStr.html)>,
    slice: [Option](https://doc.rust-lang.org/nightly/core/option/enum.Option.html)<([i64](https://doc.rust-lang.org/nightly/std/primitive.i64.html), [usize](https://doc.rust-lang.org/nightly/std/primitive.usize.html))>,
    maintain_order: [MaintainOrderJoin](../prelude/enum.MaintainOrderJoin.html),
) -> [Result](https://doc.rust-lang.org/nightly/core/result/enum.Result.html)<[DataFrame](../prelude/struct.DataFrame.html), [PolarsError](../prelude/enum.PolarsError.html)>

[Source](../../src/polars_ops/frame/join/cross_join.rs.html#47-53)

#### fn [cross_join](../prelude/trait.CrossJoin.html#method.cross_join)(
    &self,
    other: &[DataFrame](../prelude/struct.DataFrame.html),
    suffix: [Option](https://doc.rust-lang.org/nightly/core/option/enum.Option.html)<[PlSmallStr](../prelude/struct.PlSmallStr.html)>,
    slice: [Option](https://doc.rust-lang.org/nightly/core/option/enum.Option.html)<([i64](https://doc.rust-lang.org/nightly/std/primitive.i64.html), [usize](https://doc.rust-lang.org/nightly/std/primitive.usize.html))>,
    maintain_order: [MaintainOrderJoin](../prelude/enum.MaintainOrderJoin.html),
) -> [Result](https://doc.rust-lang.org/nightly/core/result/enum.Result.html)<[DataFrame](../prelude/struct.DataFrame.html), [PolarsError](../prelude/enum.PolarsError.html)>

## [Source](../../src/polars_ops/frame/is_sorted.rs.html#20)
### impl [DataFrameIsSorted](../prelude/trait.DataFrameIsSorted.html) for [DataFrame](../prelude/struct.DataFrame.html)

[Source](../../src/polars_ops/frame/is_sorted.rs.html#20)

### impl [DataFrameIsSorted](../prelude/trait.DataFrameIsSorted.html) for [DataFrame](../prelude/struct.DataFrame.html)

[Source](../../src/polars_ops/frame/is_sorted.rs.html#21-26)

#### fn [is_sorted](../prelude/trait.DataFrameIsSorted.html#tymethod.is_sorted)(
    &self,
    by: &[[PlSmallStr](../prelude/struct.PlSmallStr.html)],
    descending: &[[bool](https://doc.rust-lang.org/nightly/std/primitive.bool.html)],
    nulls_last: &[[bool](https://doc.rust-lang.org/nightly/std/primitive.bool.html)],
) -> [Result](https://doc.rust-lang.org/nightly/core/result/enum.Result.html)<[bool](https://doc.rust-lang.org/nightly/std/primitive.bool.html), [PolarsError](../prelude/enum.PolarsError.html)>

## [Source](../../src/polars_ops/frame/join/mod.rs.html#655)
### impl [DataFrameJoinOps](../prelude/trait.DataFrameJoinOps.html) for [DataFrame](../prelude/struct.DataFrame.html)

[Source](../../src/polars_ops/frame/join/mod.rs.html#655)

### impl [DataFrameJoinOps](../prelude/trait.DataFrameJoinOps.html) for [DataFrame](../prelude/struct.DataFrame.html)

## [Source](../../src/polars_ops/frame/join/mod.rs.html#90-97)
#### fn [join](../prelude/trait.DataFrameJoinOps.html#method.join)(
    &self,
    other: &[DataFrame](../prelude/struct.DataFrame.html),
    left_on: impl [IntoIterator](https://doc.rust-lang.org/nightly/core/iter/traits/collect/trait.IntoIterator.html)<Item = impl [AsRef](https://doc.rust-lang.org/nightly/core/convert/trait.AsRef.html)<[str](https://doc.rust-lang.org/nightly/std/primitive.str.html)>>,
    right_on: impl [IntoIterator](https://doc.rust-lang.org/nightly/core/iter/traits/collect/trait.IntoIterator.html)<Item = impl [AsRef](https://doc.rust-lang.org/nightly/core/convert/trait.AsRef.html)<[str](https://doc.rust-lang.org/nightly/std/primitive.str.html)>>,
    args: [JoinArgs](../prelude/struct.JoinArgs.html),
    options: [Option](https://doc.rust-lang.org/nightly/core/option/enum.Option.html)<[JoinTypeOptions](../prelude/enum.JoinTypeOptions.html)>,
) -> [Result](https://doc.rust-lang.org/nightly/core/result/enum.Result.html)<[DataFrame](../prelude/struct.DataFrame.html), [PolarsError](../prelude/enum.PolarsError.html)>

[Source](../../src/polars_ops/frame/join/mod.rs.html#90-97)

#### fn [join](../prelude/trait.DataFrameJoinOps.html#method.join)(
    &self,
    other: &[DataFrame](../prelude/struct.DataFrame.html),
    left_on: impl [IntoIterator](https://doc.rust-lang.org/nightly/core/iter/traits/collect/trait.IntoIterator.html)<Item = impl [AsRef](https://doc.rust-lang.org/nightly/core/convert/trait.AsRef.html)<[str](https://doc.rust-lang.org/nightly/std/primitive.str.html)>>,
    right_on: impl [IntoIterator](https://doc.rust-lang.org/nightly/core/iter/traits/collect/trait.IntoIterator.html)<Item = impl [AsRef](https://doc.rust-lang.org/nightly/core/convert/trait.AsRef.html)<[str](https://doc.rust-lang.org/nightly/std/primitive.str.html)>>,
    args: [JoinArgs](../prelude/struct.JoinArgs.html),
    options: [Option](https://doc.rust-lang.org/nightly/core/option/enum.Option.html)<[JoinTypeOptions](../prelude/enum.JoinTypeOptions.html)>,
) -> [Result](https://doc.rust-lang.org/nightly/core/result/enum.Result.html)<[DataFrame](../prelude/struct.DataFrame.html), [PolarsError](../prelude/enum.PolarsError.html)>

[Read more](../prelude/trait.DataFrameJoinOps.html#method.join)

## [Source](../../src/polars_ops/frame/join/mod.rs.html#472-477)
#### fn [inner_join](../prelude/trait.DataFrameJoinOps.html#method.inner_join)(
    &self,
    other: &[DataFrame](../prelude/struct.DataFrame.html),
    left_on: impl [IntoIterator](https://doc.rust-lang.org/nightly/core/iter/traits/collect/trait.IntoIterator.html)<Item = impl [AsRef](https://doc.rust-lang.org/nightly/core/convert/trait.AsRef.html)<[str](https://doc.rust-lang.org/nightly/std/primitive.str.html)>>,
    right_on: impl [IntoIterator](https://doc.rust-lang.org/nightly/core/iter/traits/collect/trait.IntoIterator.html)<Item = impl [AsRef](https://doc.rust-lang.org/nightly/core/convert/trait.AsRef.html)<[str](https://doc.rust-lang.org/nightly/std/primitive.str.html)>>,
) -> [Result](https://doc.rust-lang.org/nightly/core/result/enum.Result.html)<[DataFrame](../prelude/struct.DataFrame.html), [PolarsError](../prelude/enum.PolarsError.html)>

[Source](../../src/polars_ops/frame/join/mod.rs.html#472-477)

#### fn [inner_join](../prelude/trait.DataFrameJoinOps.html#method.inner_join)(
    &self,
    other: &[DataFrame](../prelude/struct.DataFrame.html),
    left_on: impl [IntoIterator](https://doc.rust-lang.org/nightly/core/iter/traits/collect/trait.IntoIterator.html)<Item = impl [AsRef](https://doc.rust-lang.org/nightly/core/convert/trait.AsRef.html)<[str](https://doc.rust-lang.org/nightly/std/primitive.str.html)>>,
    right_on: impl [IntoIterator](https://doc.rust-lang.org/nightly/core/iter/traits/collect/trait.IntoIterator.html)<Item = impl [AsRef](https://doc.rust-lang.org/nightly/core/convert/trait.AsRef.html)<[str](https://doc.rust-lang.org/nightly/std/primitive.str.html)>>,
) -> [Result](https://doc.rust-lang.org/nightly/core/result/enum.Result.html)<[DataFrame](../prelude/struct.DataFrame.html), [PolarsError](../prelude/enum.PolarsError.html)>

[Read more](../prelude/trait.DataFrameJoinOps.html#method.inner_join)

## [Source](../../src/polars_ops/frame/join/mod.rs.html#522-527)
#### fn [left_join](../prelude/trait.DataFrameJoinOps.html#method.left_join)(
    &self,
    other: &[DataFrame](../prelude/struct.DataFrame.html),
    left_on: impl [IntoIterator](https://doc.rust-lang.org/nightly/core/iter/traits/collect/trait.IntoIterator.html)<Item = impl [AsRef](https://doc.rust-lang.org/nightly/core/convert/trait.AsRef.html)<[str](https://doc.rust-lang.org/nightly/std/primitive.str.html)>>,
    right_on: impl [IntoIterator](https://doc.rust-lang.org/nightly/core/iter/traits/collect/trait.IntoIterator.html)<Item = impl [AsRef](https://doc.rust-lang.org/nightly/core/convert/trait.AsRef.html)<[str](https://doc.rust-lang.org/nightly/std/primitive.str.html)>>,
) -> [Result](https://doc.rust-lang.org/nightly/core/result/enum.Result.html)<[DataFrame](../prelude/struct.DataFrame.html), [PolarsError](../prelude/enum.PolarsError.html)>

[Source](../../src/polars_ops/frame/join/mod.rs.html#522-527)

#### fn [left_join](../prelude/trait.DataFrameJoinOps.html#method.left_join)(
    &self,
    other: &[DataFrame](../prelude/struct.DataFrame.html),
    left_on: impl [IntoIterator](https://doc.rust-lang.org/nightly/core/iter/traits/collect/trait.IntoIterator.html)<Item = impl [AsRef](https://doc.rust-lang.org/nightly/core/convert/trait.AsRef.html)<[str](https://doc.rust-lang.org/nightly/std/primitive.str.html)>>,
    right_on: impl [IntoIterator](https://doc.rust-lang.org/nightly/core/iter/traits/collect/trait.IntoIterator.html)<Item = impl [AsRef](https://doc.rust-lang.org/nightly/core/convert/trait.AsRef.html)<[str](https://doc.rust-lang.org/nightly/std/primitive.str.html)>>,
) -> [Result](https://doc.rust-lang.org/nightly/core/result/enum.Result.html)<[DataFrame](../prelude/struct.DataFrame.html), [PolarsError](../prelude/enum.PolarsError.html)>

[Read more](../prelude/trait.DataFrameJoinOps.html#method.left_join)

## [Source](../../src/polars_ops/frame/join/mod.rs.html#547-552)
#### fn [full_join](../prelude/trait.DataFrameJoinOps.html#method.full_join)(
    &self,
    other: &[DataFrame](../prelude/struct.DataFrame.html),
    left_on: impl [IntoIterator](https://doc.rust-lang.org/nightly/core/iter/traits/collect/trait.IntoIterator.html)<Item = impl [AsRef](https://doc.rust-lang.org/nightly/core/convert/trait.AsRef.html)<[str](https://doc.rust-lang.org/nightly/std/primitive.str.html)>>,
    right_on: impl [IntoIterator](https://doc.rust-lang.org/nightly/core/iter/traits/collect/trait.IntoIterator.html)<Item = impl [AsRef](https://doc.rust-lang.org/nightly/core/convert/trait.AsRef.html)<[str](https://doc.rust-lang.org/nightly/std/primitive.str.html)>>,
) -> [Result](https://doc.rust-lang.org/nightly/core/result/enum.Result.html)<[DataFrame](../prelude/struct.DataFrame.html), [PolarsError](../prelude/enum.PolarsError.html)>

[Source](../../src/polars_ops/frame/join/mod.rs.html#547-552)

#### fn [full_join](../prelude/trait.DataFrameJoinOps.html#method.full_join)(
    &self,
    other: &[DataFrame](../prelude/struct.DataFrame.html),
    left_on: impl [IntoIterator](https://doc.rust-lang.org/nightly/core/iter/traits/collect/trait.IntoIterator.html)<Item = impl [AsRef](https://doc.rust-lang.org/nightly/core/convert/trait.AsRef.html)<[str](https://doc.rust-lang.org/nightly/std/primitive.str.html)>>,
    right_on: impl [IntoIterator](https://doc.rust-lang.org/nightly/core/iter/traits/collect/trait.IntoIterator.html)<Item = impl [AsRef](https://doc.rust-lang.org/nightly/core/convert/trait.AsRef.html)<[str](https://doc.rust-lang.org/nightly/std/primitive.str.html)>>,
) -> [Result](https://doc.rust-lang.org/nightly/core/result/enum.Result.html)<[DataFrame](../prelude/struct.DataFrame.html), [PolarsError](../prelude/enum.PolarsError.html)>

[Read more](../prelude/trait.DataFrameJoinOps.html#method.full_join)

## [Source](../../src/polars_core/serde/df.rs.html#171)
### impl<'de> [Deserialize](https://docs.rs/serde_core/1.0.229/serde_core/de/trait.Deserialize.html)<'de> for [DataFrame](../prelude/struct.DataFrame.html)

[Source](../../src/polars_core/serde/df.rs.html#171)

### impl<'de> [Deserialize](https://docs.rs/serde_core/1.0.229/serde_core/de/trait.Deserialize.html)<'de> for [DataFrame](../prelude/struct.DataFrame.html)

## [Source](../../src/polars_core/serde/df.rs.html#172-174)
#### fn [deserialize](https://docs.rs/serde_core/1.0.229/serde_core/de/trait.Deserialize.html#tymethod.deserialize)<D>(
    deserializer: D,
) -> [Result](https://doc.rust-lang.org/nightly/core/result/enum.Result.html)<[DataFrame](../prelude/struct.DataFrame.html), <D as [Deserializer](https://docs.rs/serde_core/1.0.229/serde_core/de/trait.Deserializer.html)<'de>>::[Error](https://docs.rs/serde_core/1.0.229/serde_core/de/trait.Deserializer.html#associatedtype.Error)>where
    D: [Deserializer](https://docs.rs/serde_core/1.0.229/serde_core/de/trait.Deserializer.html)<'de>,

[Source](../../src/polars_core/serde/df.rs.html#172-174)

#### fn [deserialize](https://docs.rs/serde_core/1.0.229/serde_core/de/trait.Deserialize.html#tymethod.deserialize)<D>(
    deserializer: D,
) -> [Result](https://doc.rust-lang.org/nightly/core/result/enum.Result.html)<[DataFrame](../prelude/struct.DataFrame.html), <D as [Deserializer](https://docs.rs/serde_core/1.0.229/serde_core/de/trait.Deserializer.html)<'de>>::[Error](https://docs.rs/serde_core/1.0.229/serde_core/de/trait.Deserializer.html#associatedtype.Error)>where
    D: [Deserializer](https://docs.rs/serde_core/1.0.229/serde_core/de/trait.Deserializer.html)<'de>,

[Deserializer](https://docs.rs/serde_core/1.0.229/serde_core/de/trait.Deserializer.html)<'de>,

[Read more](https://docs.rs/serde_core/1.0.229/serde_core/de/trait.Deserialize.html#tymethod.deserialize)

## [Source](../../src/polars_ops/frame/gather.rs.html#13)
### impl [GatherDf](../../polars_ops/frame/gather/trait.GatherDf.html) for [DataFrame](../prelude/struct.DataFrame.html)

[Source](../../src/polars_ops/frame/gather.rs.html#13)

### impl [GatherDf](../../polars_ops/frame/gather/trait.GatherDf.html) for [DataFrame](../prelude/struct.DataFrame.html)

## [Source](../../src/polars_ops/frame/gather.rs.html#14)
#### fn [gather_with_column](../../polars_ops/frame/gather/trait.GatherDf.html#tymethod.gather_with_column)(
    &self,
    idxs: &[Column](../prelude/enum.Column.html),
    null_on_oob: [bool](https://doc.rust-lang.org/nightly/std/primitive.bool.html),
) -> [Result](https://doc.rust-lang.org/nightly/core/result/enum.Result.html)<[DataFrame](../prelude/struct.DataFrame.html), [PolarsError](../prelude/enum.PolarsError.html)>

[Source](../../src/polars_ops/frame/gather.rs.html#14)

#### fn [gather_with_column](../../polars_ops/frame/gather/trait.GatherDf.html#tymethod.gather_with_column)(
    &self,
    idxs: &[Column](../prelude/enum.Column.html),
    null_on_oob: [bool](https://doc.rust-lang.org/nightly/std/primitive.bool.html),
) -> [Result](https://doc.rust-lang.org/nightly/core/result/enum.Result.html)<[DataFrame](../prelude/struct.DataFrame.html), [PolarsError](../prelude/enum.PolarsError.html)>

## [Source](../../src/polars_ops/frame/gather.rs.html#32)
#### fn [gather_with_series](../../polars_ops/frame/gather/trait.GatherDf.html#tymethod.gather_with_series)(
    &self,
    idxs: &[Series](../prelude/struct.Series.html),
    null_on_oob: [bool](https://doc.rust-lang.org/nightly/std/primitive.bool.html),
) -> [Result](https://doc.rust-lang.org/nightly/core/result/enum.Result.html)<[DataFrame](../prelude/struct.DataFrame.html), [PolarsError](../prelude/enum.PolarsError.html)>

[Source](../../src/polars_ops/frame/gather.rs.html#32)

#### fn [gather_with_series](../../polars_ops/frame/gather/trait.GatherDf.html#tymethod.gather_with_series)(
    &self,
    idxs: &[Series](../prelude/struct.Series.html),
    null_on_oob: [bool](https://doc.rust-lang.org/nightly/std/primitive.bool.html),
) -> [Result](https://doc.rust-lang.org/nightly/core/result/enum.Result.html)<[DataFrame](../prelude/struct.DataFrame.html), [PolarsError](../prelude/enum.PolarsError.html)>

## [Source](../../src/polars_ops/frame/join/hash_join/mod.rs.html#227)
### impl [JoinDispatch](../prelude/trait.JoinDispatch.html) for [DataFrame](../prelude/struct.DataFrame.html)

[Source](../../src/polars_ops/frame/join/hash_join/mod.rs.html#227)

### impl [JoinDispatch](../prelude/trait.JoinDispatch.html) for [DataFrame](../prelude/struct.DataFrame.html)

## [Source](../../src/polars_ops/frame/join/hash_join/mod.rs.html#60-65)
#### unsafe fn [create_left_df_chunked](../prelude/trait.JoinDispatch.html#method.create_left_df_chunked)(
    &self,
    chunk_ids: &[[ChunkId](../prelude/struct.ChunkId.html)],
    left_join: [bool](https://doc.rust-lang.org/nightly/std/primitive.bool.html),
    was_sliced: [bool](https://doc.rust-lang.org/nightly/std/primitive.bool.html),
) -> [DataFrame](../prelude/struct.DataFrame.html)

[Source](../../src/polars_ops/frame/join/hash_join/mod.rs.html#60-65)

#### unsafe fn [create_left_df_chunked](../prelude/trait.JoinDispatch.html#method.create_left_df_chunked)(
    &self,
    chunk_ids: &[[ChunkId](../prelude/struct.ChunkId.html)],
    left_join: [bool](https://doc.rust-lang.org/nightly/std/primitive.bool.html),
    was_sliced: [bool](https://doc.rust-lang.org/nightly/std/primitive.bool.html),
) -> [DataFrame](../prelude/struct.DataFrame.html)

**crate feature**only.

`chunked_ids`
[Read more](../prelude/trait.JoinDispatch.html#method.create_left_df_chunked)

## [Source](../../src/polars_ops/frame/join/hash_join/mod.rs.html#86-92)
#### unsafe fn [_create_left_df_from_slice](../prelude/trait.JoinDispatch.html#method._create_left_df_from_slice)(
    &self,
    join_tuples: &[[u32](https://doc.rust-lang.org/nightly/std/primitive.u32.html)],
    left_join: [bool](https://doc.rust-lang.org/nightly/std/primitive.bool.html),
    was_sliced: [bool](https://doc.rust-lang.org/nightly/std/primitive.bool.html),
    sorted_tuple_idx: [bool](https://doc.rust-lang.org/nightly/std/primitive.bool.html),
) -> [DataFrame](../prelude/struct.DataFrame.html)

[Source](../../src/polars_ops/frame/join/hash_join/mod.rs.html#86-92)

#### unsafe fn [_create_left_df_from_slice](../prelude/trait.JoinDispatch.html#method._create_left_df_from_slice)(
    &self,
    join_tuples: &[[u32](https://doc.rust-lang.org/nightly/std/primitive.u32.html)],
    left_join: [bool](https://doc.rust-lang.org/nightly/std/primitive.bool.html),
    was_sliced: [bool](https://doc.rust-lang.org/nightly/std/primitive.bool.html),
    sorted_tuple_idx: [bool](https://doc.rust-lang.org/nightly/std/primitive.bool.html),
) -> [DataFrame](../prelude/struct.DataFrame.html)

[Read more](../prelude/trait.JoinDispatch.html#method._create_left_df_from_slice)

## [Source](../../src/polars_ops/frame/join/hash_join/mod.rs.html#114-118)
#### unsafe fn [_finish_anti_semi_join](../prelude/trait.JoinDispatch.html#method._finish_anti_semi_join)(
    &self,
    idx: &[[u32](https://doc.rust-lang.org/nightly/std/primitive.u32.html)],
    slice: [Option](https://doc.rust-lang.org/nightly/core/option/enum.Option.html)<([i64](https://doc.rust-lang.org/nightly/std/primitive.i64.html), [usize](https://doc.rust-lang.org/nightly/std/primitive.usize.html))>,
) -> [DataFrame](../prelude/struct.DataFrame.html)

[Source](../../src/polars_ops/frame/join/hash_join/mod.rs.html#114-118)

#### unsafe fn [_finish_anti_semi_join](../prelude/trait.JoinDispatch.html#method._finish_anti_semi_join)(
    &self,
    idx: &[[u32](https://doc.rust-lang.org/nightly/std/primitive.u32.html)],
    slice: [Option](https://doc.rust-lang.org/nightly/core/option/enum.Option.html)<([i64](https://doc.rust-lang.org/nightly/std/primitive.i64.html), [usize](https://doc.rust-lang.org/nightly/std/primitive.usize.html))>,
) -> [DataFrame](../prelude/struct.DataFrame.html)

**crate feature**only.

`semi_anti_join`
[Read more](../prelude/trait.JoinDispatch.html#method._finish_anti_semi_join)

## [Source](../../src/polars_ops/frame/join/hash_join/mod.rs.html#128-135)
#### fn [_semi_anti_join_from_series](../prelude/trait.JoinDispatch.html#method._semi_anti_join_from_series)(
    &self,
    s_left: &[Series](../prelude/struct.Series.html),
    s_right: &[Series](../prelude/struct.Series.html),
    slice: [Option](https://doc.rust-lang.org/nightly/core/option/enum.Option.html)<([i64](https://doc.rust-lang.org/nightly/std/primitive.i64.html), [usize](https://doc.rust-lang.org/nightly/std/primitive.usize.html))>,
    anti: [bool](https://doc.rust-lang.org/nightly/std/primitive.bool.html),
    nulls_equal: [bool](https://doc.rust-lang.org/nightly/std/primitive.bool.html),
) -> [Result](https://doc.rust-lang.org/nightly/core/result/enum.Result.html)<[DataFrame](../prelude/struct.DataFrame.html), [PolarsError](../prelude/enum.PolarsError.html)>

[Source](../../src/polars_ops/frame/join/hash_join/mod.rs.html#128-135)

#### fn [_semi_anti_join_from_series](../prelude/trait.JoinDispatch.html#method._semi_anti_join_from_series)(
    &self,
    s_left: &[Series](../prelude/struct.Series.html),
    s_right: &[Series](../prelude/struct.Series.html),
    slice: [Option](https://doc.rust-lang.org/nightly/core/option/enum.Option.html)<([i64](https://doc.rust-lang.org/nightly/std/primitive.i64.html), [usize](https://doc.rust-lang.org/nightly/std/primitive.usize.html))>,
    anti: [bool](https://doc.rust-lang.org/nightly/std/primitive.bool.html),
    nulls_equal: [bool](https://doc.rust-lang.org/nightly/std/primitive.bool.html),
) -> [Result](https://doc.rust-lang.org/nightly/core/result/enum.Result.html)<[DataFrame](../prelude/struct.DataFrame.html), [PolarsError](../prelude/enum.PolarsError.html)>

**crate feature**only.

`semi_anti_join`
[Source](../../src/polars_ops/frame/join/hash_join/mod.rs.html#143-149)

#### fn [_full_join_from_series](../prelude/trait.JoinDispatch.html#method._full_join_from_series)(
    &self,
    other: &[DataFrame](../prelude/struct.DataFrame.html),
    s_left: &[Series](../prelude/struct.Series.html),
    s_right: &[Series](../prelude/struct.Series.html),
    args: [JoinArgs](../prelude/struct.JoinArgs.html),
) -> [Result](https://doc.rust-lang.org/nightly/core/result/enum.Result.html)<[DataFrame](../prelude/struct.DataFrame.html), [PolarsError](../prelude/enum.PolarsError.html)>

## [Source](../../src/polars_ops/series/ops/horizontal.rs.html#20)
### impl [MinMaxHorizontal](../prelude/trait.MinMaxHorizontal.html) for [DataFrame](../prelude/struct.DataFrame.html)

[Source](../../src/polars_ops/series/ops/horizontal.rs.html#20)

### impl [MinMaxHorizontal](../prelude/trait.MinMaxHorizontal.html) for [DataFrame](../prelude/struct.DataFrame.html)

## [Source](../../src/polars_ops/series/ops/horizontal.rs.html#21)
#### fn [min_horizontal](../prelude/trait.MinMaxHorizontal.html#tymethod.min_horizontal)(&self) -> [Result](https://doc.rust-lang.org/nightly/core/result/enum.Result.html)<[Option](https://doc.rust-lang.org/nightly/core/option/enum.Option.html)<[Column](../prelude/enum.Column.html)>, [PolarsError](../prelude/enum.PolarsError.html)>

[Source](../../src/polars_ops/series/ops/horizontal.rs.html#21)

#### fn [min_horizontal](../prelude/trait.MinMaxHorizontal.html#tymethod.min_horizontal)(&self) -> [Result](https://doc.rust-lang.org/nightly/core/result/enum.Result.html)<[Option](https://doc.rust-lang.org/nightly/core/option/enum.Option.html)<[Column](../prelude/enum.Column.html)>, [PolarsError](../prelude/enum.PolarsError.html)>

## [Source](../../src/polars_ops/series/ops/horizontal.rs.html#24)
#### fn [max_horizontal](../prelude/trait.MinMaxHorizontal.html#tymethod.max_horizontal)(&self) -> [Result](https://doc.rust-lang.org/nightly/core/result/enum.Result.html)<[Option](https://doc.rust-lang.org/nightly/core/option/enum.Option.html)<[Column](../prelude/enum.Column.html)>, [PolarsError](../prelude/enum.PolarsError.html)>

[Source](../../src/polars_ops/series/ops/horizontal.rs.html#24)

#### fn [max_horizontal](../prelude/trait.MinMaxHorizontal.html#tymethod.max_horizontal)(&self) -> [Result](https://doc.rust-lang.org/nightly/core/result/enum.Result.html)<[Option](https://doc.rust-lang.org/nightly/core/option/enum.Option.html)<[Column](../prelude/enum.Column.html)>, [PolarsError](../prelude/enum.PolarsError.html)>

## [Source](../../src/polars_time/group_by/dynamic.rs.html#99)
### impl [PolarsTemporalGroupby](../prelude/trait.PolarsTemporalGroupby.html) for [DataFrame](../prelude/struct.DataFrame.html)

[Source](../../src/polars_time/group_by/dynamic.rs.html#99)

### impl [PolarsTemporalGroupby](../prelude/trait.PolarsTemporalGroupby.html) for [DataFrame](../prelude/struct.DataFrame.html)

[Source](../../src/polars_time/group_by/dynamic.rs.html#100-104)

#### fn [rolling](../prelude/trait.PolarsTemporalGroupby.html#tymethod.rolling)(
    &self,
    group_by: [Option](https://doc.rust-lang.org/nightly/core/option/enum.Option.html)<[Vec](https://doc.rust-lang.org/nightly/alloc/vec/struct.Vec.html)<[[u32](https://doc.rust-lang.org/nightly/std/primitive.u32.html); [2](https://doc.rust-lang.org/nightly/std/primitive.array.html)]>>,
    options: &[RollingGroupOptions](../prelude/struct.RollingGroupOptions.html),
) -> [Result](https://doc.rust-lang.org/nightly/core/result/enum.Result.html)<([Column](../prelude/enum.Column.html), [GroupPositions](../prelude/struct.GroupPositions.html)), [PolarsError](../prelude/enum.PolarsError.html)>

[Source](../../src/polars_time/group_by/dynamic.rs.html#108-112)

#### fn [group_by_dynamic](../prelude/trait.PolarsTemporalGroupby.html#tymethod.group_by_dynamic)(
    &self,
    group_by: [Option](https://doc.rust-lang.org/nightly/core/option/enum.Option.html)<[Vec](https://doc.rust-lang.org/nightly/alloc/vec/struct.Vec.html)<[[u32](https://doc.rust-lang.org/nightly/std/primitive.u32.html); [2](https://doc.rust-lang.org/nightly/std/primitive.array.html)]>>,
    options: &[DynamicGroupOptions](../prelude/struct.DynamicGroupOptions.html),
) -> [Result](https://doc.rust-lang.org/nightly/core/result/enum.Result.html)<([Column](../prelude/enum.Column.html), [Vec](https://doc.rust-lang.org/nightly/alloc/vec/struct.Vec.html)<[Column](../prelude/enum.Column.html)>, [GroupPositions](../prelude/struct.GroupPositions.html)), [PolarsError](../prelude/enum.PolarsError.html)>

## [Source](../../src/polars_time/upsample.rs.html#91)
### impl [PolarsUpsample](../prelude/trait.PolarsUpsample.html) for [DataFrame](../prelude/struct.DataFrame.html)

[Source](../../src/polars_time/upsample.rs.html#91)

### impl [PolarsUpsample](../prelude/trait.PolarsUpsample.html) for [DataFrame](../prelude/struct.DataFrame.html)

## [Source](../../src/polars_time/upsample.rs.html#92-97)
#### fn [upsample](../prelude/trait.PolarsUpsample.html#tymethod.upsample)<I>(
    &self,
    by: I,
    time_column: &[str](https://doc.rust-lang.org/nightly/std/primitive.str.html),
    every: [Duration](../prelude/struct.Duration.html),
) -> [Result](https://doc.rust-lang.org/nightly/core/result/enum.Result.html)<[DataFrame](../prelude/struct.DataFrame.html), [PolarsError](../prelude/enum.PolarsError.html)>where
    I: [IntoVec](../prelude/trait.IntoVec.html)<[PlSmallStr](../prelude/struct.PlSmallStr.html)>,

[Source](../../src/polars_time/upsample.rs.html#92-97)

#### fn [upsample](../prelude/trait.PolarsUpsample.html#tymethod.upsample)<I>(
    &self,
    by: I,
    time_column: &[str](https://doc.rust-lang.org/nightly/std/primitive.str.html),
    every: [Duration](../prelude/struct.Duration.html),
) -> [Result](https://doc.rust-lang.org/nightly/core/result/enum.Result.html)<[DataFrame](../prelude/struct.DataFrame.html), [PolarsError](../prelude/enum.PolarsError.html)>where
    I: [IntoVec](../prelude/trait.IntoVec.html)<[PlSmallStr](../prelude/struct.PlSmallStr.html)>,

[IntoVec](../prelude/trait.IntoVec.html)<

[PlSmallStr](../prelude/struct.PlSmallStr.html)>,

## [Source](../../src/polars_time/upsample.rs.html#104-109)
#### fn [upsample_stable](../prelude/trait.PolarsUpsample.html#tymethod.upsample_stable)<I>(
    &self,
    by: I,
    time_column: &[str](https://doc.rust-lang.org/nightly/std/primitive.str.html),
    every: [Duration](../prelude/struct.Duration.html),
) -> [Result](https://doc.rust-lang.org/nightly/core/result/enum.Result.html)<[DataFrame](../prelude/struct.DataFrame.html), [PolarsError](../prelude/enum.PolarsError.html)>where
    I: [IntoVec](../prelude/trait.IntoVec.html)<[PlSmallStr](../prelude/struct.PlSmallStr.html)>,

[Source](../../src/polars_time/upsample.rs.html#104-109)

#### fn [upsample_stable](../prelude/trait.PolarsUpsample.html#tymethod.upsample_stable)<I>(
    &self,
    by: I,
    time_column: &[str](https://doc.rust-lang.org/nightly/std/primitive.str.html),
    every: [Duration](../prelude/struct.Duration.html),
) -> [Result](https://doc.rust-lang.org/nightly/core/result/enum.Result.html)<[DataFrame](../prelude/struct.DataFrame.html), [PolarsError](../prelude/enum.PolarsError.html)>where
    I: [IntoVec](../prelude/trait.IntoVec.html)<[PlSmallStr](../prelude/struct.PlSmallStr.html)>,

[IntoVec](../prelude/trait.IntoVec.html)<

[PlSmallStr](../prelude/struct.PlSmallStr.html)>,

## [Source](../../src/polars_core/serde/df.rs.html#155)
### impl [Serialize](https://docs.rs/serde_core/1.0.229/serde_core/ser/trait.Serialize.html) for [DataFrame](../prelude/struct.DataFrame.html)

[Source](../../src/polars_core/serde/df.rs.html#155)

### impl [Serialize](https://docs.rs/serde_core/1.0.229/serde_core/ser/trait.Serialize.html) for [DataFrame](../prelude/struct.DataFrame.html)

## [Source](../../src/polars_core/serde/df.rs.html#156-158)
#### fn [serialize](https://docs.rs/serde_core/1.0.229/serde_core/ser/trait.Serialize.html#tymethod.serialize)<S>(
    &self,
    serializer: S,
) -> [Result](https://doc.rust-lang.org/nightly/core/result/enum.Result.html)<<S as [Serializer](https://docs.rs/serde_core/1.0.229/serde_core/ser/trait.Serializer.html)>::[Ok](https://docs.rs/serde_core/1.0.229/serde_core/ser/trait.Serializer.html#associatedtype.Ok), <S as [Serializer](https://docs.rs/serde_core/1.0.229/serde_core/ser/trait.Serializer.html)>::[Error](https://docs.rs/serde_core/1.0.229/serde_core/ser/trait.Serializer.html#associatedtype.Error)>where
    S: [Serializer](https://docs.rs/serde_core/1.0.229/serde_core/ser/trait.Serializer.html),

[Source](../../src/polars_core/serde/df.rs.html#156-158)

#### fn [serialize](https://docs.rs/serde_core/1.0.229/serde_core/ser/trait.Serialize.html#tymethod.serialize)<S>(
    &self,
    serializer: S,
) -> [Result](https://doc.rust-lang.org/nightly/core/result/enum.Result.html)<<S as [Serializer](https://docs.rs/serde_core/1.0.229/serde_core/ser/trait.Serializer.html)>::[Ok](https://docs.rs/serde_core/1.0.229/serde_core/ser/trait.Serializer.html#associatedtype.Ok), <S as [Serializer](https://docs.rs/serde_core/1.0.229/serde_core/ser/trait.Serializer.html)>::[Error](https://docs.rs/serde_core/1.0.229/serde_core/ser/trait.Serializer.html#associatedtype.Error)>where
    S: [Serializer](https://docs.rs/serde_core/1.0.229/serde_core/ser/trait.Serializer.html),

[Serializer](https://docs.rs/serde_core/1.0.229/serde_core/ser/trait.Serializer.html),

[Read more](https://docs.rs/serde_core/1.0.229/serde_core/ser/trait.Serialize.html#tymethod.serialize)

##
### impl Spillable for [DataFrame](../prelude/struct.DataFrame.html)

### impl Spillable for [DataFrame](../prelude/struct.DataFrame.html)

## [Source](../../src/polars_ops/series/ops/horizontal.rs.html#43)
### impl [SumMeanHorizontal](../prelude/trait.SumMeanHorizontal.html) for [DataFrame](../prelude/struct.DataFrame.html)

[Source](../../src/polars_ops/series/ops/horizontal.rs.html#43)

### impl [SumMeanHorizontal](../prelude/trait.SumMeanHorizontal.html) for [DataFrame](../prelude/struct.DataFrame.html)

## [Source](../../src/polars_ops/series/ops/horizontal.rs.html#44)
#### fn [sum_horizontal](../prelude/trait.SumMeanHorizontal.html#tymethod.sum_horizontal)(
    &self,
    null_strategy: [NullStrategy](../prelude/enum.NullStrategy.html),
) -> [Result](https://doc.rust-lang.org/nightly/core/result/enum.Result.html)<[Option](https://doc.rust-lang.org/nightly/core/option/enum.Option.html)<[Column](../prelude/enum.Column.html)>, [PolarsError](../prelude/enum.PolarsError.html)>

[Source](../../src/polars_ops/series/ops/horizontal.rs.html#44)

#### fn [sum_horizontal](../prelude/trait.SumMeanHorizontal.html#tymethod.sum_horizontal)(
    &self,
    null_strategy: [NullStrategy](../prelude/enum.NullStrategy.html),
) -> [Result](https://doc.rust-lang.org/nightly/core/result/enum.Result.html)<[Option](https://doc.rust-lang.org/nightly/core/option/enum.Option.html)<[Column](../prelude/enum.Column.html)>, [PolarsError](../prelude/enum.PolarsError.html)>

## [Source](../../src/polars_ops/series/ops/horizontal.rs.html#47)
#### fn [mean_horizontal](../prelude/trait.SumMeanHorizontal.html#tymethod.mean_horizontal)(
    &self,
    null_strategy: [NullStrategy](../prelude/enum.NullStrategy.html),
) -> [Result](https://doc.rust-lang.org/nightly/core/result/enum.Result.html)<[Option](https://doc.rust-lang.org/nightly/core/option/enum.Option.html)<[Column](../prelude/enum.Column.html)>, [PolarsError](../prelude/enum.PolarsError.html)>

[Source](../../src/polars_ops/series/ops/horizontal.rs.html#47)

#### fn [mean_horizontal](../prelude/trait.SumMeanHorizontal.html#tymethod.mean_horizontal)(
    &self,
    null_strategy: [NullStrategy](../prelude/enum.NullStrategy.html),
) -> [Result](https://doc.rust-lang.org/nightly/core/result/enum.Result.html)<[Option](https://doc.rust-lang.org/nightly/core/option/enum.Option.html)<[Column](../prelude/enum.Column.html)>, [PolarsError](../prelude/enum.PolarsError.html)>

## [Source](../../src/polars_ops/chunked_array/gather/chunked.rs.html#42)
### impl [TakeChunked](../prelude/trait.TakeChunked.html) for [DataFrame](../prelude/struct.DataFrame.html)

[Source](../../src/polars_ops/chunked_array/gather/chunked.rs.html#42)

### impl [TakeChunked](../prelude/trait.TakeChunked.html) for [DataFrame](../prelude/struct.DataFrame.html)

## [Source](../../src/polars_ops/chunked_array/gather/chunked.rs.html#109)
### impl [TakeChunkedHorPar](../prelude/trait.TakeChunkedHorPar.html) for [DataFrame](../prelude/struct.DataFrame.html)

[Source](../../src/polars_ops/chunked_array/gather/chunked.rs.html#109)

### impl [TakeChunkedHorPar](../prelude/trait.TakeChunkedHorPar.html) for [DataFrame](../prelude/struct.DataFrame.html)

## [Source](../../src/polars_core/frame/upstream_traits.rs.html#7)
### impl TryExtend<RecordBatchT<[Box](https://doc.rust-lang.org/nightly/alloc/boxed/struct.Box.html)<dyn Array>>> for [DataFrame](../prelude/struct.DataFrame.html)

[Source](../../src/polars_core/frame/upstream_traits.rs.html#7)

### impl TryExtend<RecordBatchT<[Box](https://doc.rust-lang.org/nightly/alloc/boxed/struct.Box.html)<dyn Array>>> for [DataFrame](../prelude/struct.DataFrame.html)

## [Source](../../src/polars_core/frame/upstream_traits.rs.html#8-11)
#### fn [try_extend]<I>(&mut self, iter: I) -> [Result](https://doc.rust-lang.org/nightly/core/result/enum.Result.html)<[()](https://doc.rust-lang.org/nightly/std/primitive.unit.html), [PolarsError](../prelude/enum.PolarsError.html)>where
    I: [IntoIterator](https://doc.rust-lang.org/nightly/core/iter/traits/collect/trait.IntoIterator.html)<Item = RecordBatchT<[Box](https://doc.rust-lang.org/nightly/alloc/boxed/struct.Box.html)<dyn Array>>>,

[Source](../../src/polars_core/frame/upstream_traits.rs.html#8-11)

#### fn [try_extend]<I>(&mut self, iter: I) -> [Result](https://doc.rust-lang.org/nightly/core/result/enum.Result.html)<[()](https://doc.rust-lang.org/nightly/std/primitive.unit.html), [PolarsError](../prelude/enum.PolarsError.html)>where
    I: [IntoIterator](https://doc.rust-lang.org/nightly/core/iter/traits/collect/trait.IntoIterator.html)<Item = RecordBatchT<[Box](https://doc.rust-lang.org/nightly/alloc/boxed/struct.Box.html)<dyn Array>>>,

[IntoIterator](https://doc.rust-lang.org/nightly/core/iter/traits/collect/trait.IntoIterator.html)<Item = RecordBatchT<

[Box](https://doc.rust-lang.org/nightly/alloc/boxed/struct.Box.html)<dyn Array>>>,

[.](https://doc.rust-lang.org/nightly/core/iter/traits/collect/trait.Extend.html#tymethod.extend)

`Extend::extend`
## [Source](../../src/polars_core/frame/upstream_traits.rs.html#20)
### impl TryExtend<[Result](https://doc.rust-lang.org/nightly/core/result/enum.Result.html)<RecordBatchT<[Box](https://doc.rust-lang.org/nightly/alloc/boxed/struct.Box.html)<dyn Array>>, [PolarsError](../prelude/enum.PolarsError.html)>> for [DataFrame](../prelude/struct.DataFrame.html)

[Source](../../src/polars_core/frame/upstream_traits.rs.html#20)

### impl TryExtend<[Result](https://doc.rust-lang.org/nightly/core/result/enum.Result.html)<RecordBatchT<[Box](https://doc.rust-lang.org/nightly/alloc/boxed/struct.Box.html)<dyn Array>>, [PolarsError](../prelude/enum.PolarsError.html)>> for [DataFrame](../prelude/struct.DataFrame.html)

## [Source](../../src/polars_core/frame/upstream_traits.rs.html#21-24)
#### fn [try_extend]<I>(&mut self, iter: I) -> [Result](https://doc.rust-lang.org/nightly/core/result/enum.Result.html)<[()](https://doc.rust-lang.org/nightly/std/primitive.unit.html), [PolarsError](../prelude/enum.PolarsError.html)>

[Source](../../src/polars_core/frame/upstream_traits.rs.html#21-24)

#### fn [try_extend]<I>(&mut self, iter: I) -> [Result](https://doc.rust-lang.org/nightly/core/result/enum.Result.html)<[()](https://doc.rust-lang.org/nightly/std/primitive.unit.html), [PolarsError](../prelude/enum.PolarsError.html)>

[.](https://doc.rust-lang.org/nightly/core/iter/traits/collect/trait.Extend.html#tymethod.extend)

`Extend::extend`
## [Source](../../src/polars_core/frame/from.rs.html#3)
### impl [TryFrom](https://doc.rust-lang.org/nightly/core/convert/trait.TryFrom.html)<[StructArray](../prelude/struct.StructArray.html)> for [DataFrame](../prelude/struct.DataFrame.html)

[Source](../../src/polars_core/frame/from.rs.html#3)

### impl [TryFrom](https://doc.rust-lang.org/nightly/core/convert/trait.TryFrom.html)<[StructArray](../prelude/struct.StructArray.html)> for [DataFrame](../prelude/struct.DataFrame.html)

## [Source](../../src/polars_core/frame/from.rs.html#4)
#### type [Error](https://doc.rust-lang.org/nightly/core/convert/trait.TryFrom.html#associatedtype.Error) = [PolarsError](../prelude/enum.PolarsError.html)

[Source](../../src/polars_core/frame/from.rs.html#4)

#### type [Error](https://doc.rust-lang.org/nightly/core/convert/trait.TryFrom.html#associatedtype.Error) = [PolarsError](../prelude/enum.PolarsError.html)

## [Source](../../src/polars_core/frame/from.rs.html#6)
#### fn [try_from](https://doc.rust-lang.org/nightly/core/convert/trait.TryFrom.html#tymethod.try_from)(arr: [StructArray](../prelude/struct.StructArray.html)) -> [Result](https://doc.rust-lang.org/nightly/core/result/enum.Result.html)<[DataFrame](../prelude/struct.DataFrame.html), [PolarsError](../prelude/enum.PolarsError.html)>

[Source](../../src/polars_core/frame/from.rs.html#6)

#### fn [try_from](https://doc.rust-lang.org/nightly/core/convert/trait.TryFrom.html#tymethod.try_from)(arr: [StructArray](../prelude/struct.StructArray.html)) -> [Result](https://doc.rust-lang.org/nightly/core/result/enum.Result.html)<[DataFrame](../prelude/struct.DataFrame.html), [PolarsError](../prelude/enum.PolarsError.html)>

## Auto Trait Implementations

### impl ![Freeze](https://doc.rust-lang.org/nightly/core/marker/trait.Freeze.html) for [DataFrame](../prelude/struct.DataFrame.html)

### impl ![RefUnwindSafe](https://doc.rust-lang.org/nightly/core/panic/unwind_safe/trait.RefUnwindSafe.html) for [DataFrame](../prelude/struct.DataFrame.html)

### impl ![UnwindSafe](https://doc.rust-lang.org/nightly/core/panic/unwind_safe/trait.UnwindSafe.html) for [DataFrame](../prelude/struct.DataFrame.html)

### impl [Send](https://doc.rust-lang.org/nightly/core/marker/trait.Send.html) for [DataFrame](../prelude/struct.DataFrame.html)

### impl [Sync](https://doc.rust-lang.org/nightly/core/marker/trait.Sync.html) for [DataFrame](../prelude/struct.DataFrame.html)

### impl [Unpin](https://doc.rust-lang.org/nightly/core/marker/trait.Unpin.html) for [DataFrame](../prelude/struct.DataFrame.html)

### impl [UnsafeUnpin](https://doc.rust-lang.org/nightly/core/marker/trait.UnsafeUnpin.html) for [DataFrame](../prelude/struct.DataFrame.html)

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

### impl<ST, DT> CastableFrom<ST, Initialized, Initialized> for DT

### impl<ST, DT> CastableFrom<ST, Uninit, Uninit> for DT

## [Source](https://doc.rust-lang.org/nightly/src/core/clone.rs.html#648)
### impl<T> [CloneToUninit](https://doc.rust-lang.org/nightly/core/clone/trait.CloneToUninit.html) for Twhere
    T: [Clone](https://doc.rust-lang.org/nightly/core/clone/trait.Clone.html),

[Source](https://doc.rust-lang.org/nightly/src/core/clone.rs.html#648)

### impl<T> [CloneToUninit](https://doc.rust-lang.org/nightly/core/clone/trait.CloneToUninit.html) for Twhere
    T: [Clone](https://doc.rust-lang.org/nightly/core/clone/trait.Clone.html),

[Clone](https://doc.rust-lang.org/nightly/core/clone/trait.Clone.html),

## [Source](../../src/polars_ops/frame/mod.rs.html#26)
### impl<T> [DataFrameOps](../prelude/trait.DataFrameOps.html) for Twhere
    T: [IntoDf](../../polars_ops/frame/trait.IntoDf.html),

[Source](../../src/polars_ops/frame/mod.rs.html#26)

### impl<T> [DataFrameOps](../prelude/trait.DataFrameOps.html) for Twhere
    T: [IntoDf](../../polars_ops/frame/trait.IntoDf.html),

[IntoDf](../../polars_ops/frame/trait.IntoDf.html),

## [Source](../../src/polars_ops/frame/mod.rs.html#75-80)
#### fn [to_dummies](../prelude/trait.DataFrameOps.html#method.to_dummies)(
    &self,
    separator: [Option](https://doc.rust-lang.org/nightly/core/option/enum.Option.html)<&[str](https://doc.rust-lang.org/nightly/std/primitive.str.html)>,
    drop_first: [bool](https://doc.rust-lang.org/nightly/std/primitive.bool.html),
    drop_nulls: [bool](https://doc.rust-lang.org/nightly/std/primitive.bool.html),
) -> [Result](https://doc.rust-lang.org/nightly/core/result/enum.Result.html)<[DataFrame](../prelude/struct.DataFrame.html), [PolarsError](../prelude/enum.PolarsError.html)>

[Source](../../src/polars_ops/frame/mod.rs.html#75-80)

#### fn [to_dummies](../prelude/trait.DataFrameOps.html#method.to_dummies)(
    &self,
    separator: [Option](https://doc.rust-lang.org/nightly/core/option/enum.Option.html)<&[str](https://doc.rust-lang.org/nightly/std/primitive.str.html)>,
    drop_first: [bool](https://doc.rust-lang.org/nightly/std/primitive.bool.html),
    drop_nulls: [bool](https://doc.rust-lang.org/nightly/std/primitive.bool.html),
) -> [Result](https://doc.rust-lang.org/nightly/core/result/enum.Result.html)<[DataFrame](../prelude/struct.DataFrame.html), [PolarsError](../prelude/enum.PolarsError.html)>

**crate feature**only.

`to_dummies`
[Read more](../prelude/trait.DataFrameOps.html#method.to_dummies)

[Source](https://docs.rs/serde_core/1.0.229/src/serde_core/de/mod.rs.html#633)

### impl<T> [DeserializeOwned](https://docs.rs/serde_core/1.0.229/serde_core/de/trait.DeserializeOwned.html) for Twhere
    T: for<'de> [Deserialize](https://docs.rs/serde_core/1.0.229/serde_core/de/trait.Deserialize.html)<'de>,

[Deserialize](https://docs.rs/serde_core/1.0.229/serde_core/de/trait.Deserialize.html)<'de>,

##
### impl<T> Instrument for T

### impl<T> Instrument for T

##
#### fn [instrument](self, span: Span) -> Instrumented<Self> [ⓘ](#)

#### fn [instrument](self, span: Span) -> Instrumented<Self> [ⓘ](#)

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

### impl<T> PlanCallbackArgs for T

### impl<T> PlanCallbackOut for T

##
### impl<T> Pointable for T

### impl<T> Pointable for T

##
### impl<T> PolicyExt for Twhere
    T: ?[Sized](https://doc.rust-lang.org/nightly/core/marker/trait.Sized.html),

### impl<T> PolicyExt for Twhere
    T: ?[Sized](https://doc.rust-lang.org/nightly/core/marker/trait.Sized.html),

[Sized](https://doc.rust-lang.org/nightly/core/marker/trait.Sized.html),

### impl<T> Read<Exclusive, BecauseExclusive> for Twhere
    T: ?[Sized](https://doc.rust-lang.org/nightly/core/marker/trait.Sized.html),

[Sized](https://doc.rust-lang.org/nightly/core/marker/trait.Sized.html),

##
### impl<T> ToCompactString for Twhere
    T: [Display](https://doc.rust-lang.org/nightly/core/fmt/trait.Display.html),

### impl<T> ToCompactString for Twhere
    T: [Display](https://doc.rust-lang.org/nightly/core/fmt/trait.Display.html),

[Display](https://doc.rust-lang.org/nightly/core/fmt/trait.Display.html),

##
#### fn [try_to_compact_string](&self) -> [Result](https://doc.rust-lang.org/nightly/core/result/enum.Result.html)<CompactString, ToCompactStringError>

#### fn [try_to_compact_string](&self) -> [Result](https://doc.rust-lang.org/nightly/core/result/enum.Result.html)<CompactString, ToCompactStringError>

`ToCompactString::to_compact_string()`] [Read more]

##
#### fn [to_compact_string](&self) -> CompactString

#### fn [to_compact_string](&self) -> CompactString

`CompactString`]. [Read more]

##
### impl<T> WithSubscriber for T

### impl<T> WithSubscriber for T

##
#### fn [with_subscriber]<S>(self, subscriber: S) -> WithDispatch<Self> [ⓘ](#)where
    S: [Into](https://doc.rust-lang.org/nightly/core/convert/trait.Into.html)<Dispatch>,

#### fn [with_subscriber]<S>(self, subscriber: S) -> WithDispatch<Self> [ⓘ](#)where
    S: [Into](https://doc.rust-lang.org/nightly/core/convert/trait.Into.html)<Dispatch>,

[Into](https://doc.rust-lang.org/nightly/core/convert/trait.Into.html)<Dispatch>,

##
#### fn [with_current_subscriber](self) -> WithDispatch<Self> [ⓘ](#)

# Citations

1. Source page: https://docs.pola.rs/api/rust/dev/polars/frame/struct.DataFrame.html
