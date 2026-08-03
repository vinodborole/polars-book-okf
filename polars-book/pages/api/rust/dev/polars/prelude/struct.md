---
type: Web Page
title: LazyCsvReader in polars::prelude - Rust
description: API documentation for the Rust `LazyCsvReader` struct in crate `polars`.
resource: https://docs.pola.rs/api/rust/dev/polars/prelude/struct.LazyCsvReader.html
timestamp: '2026-08-03T09:49:29.273788+00:00'
---

`pub struct LazyCsvReader { /* private fields */ }`
**crate features**only.

`lazy` and `csv`
## Implementations

## [Source](../../src/polars_lazy/scan/csv.rs.html#32)
### impl [LazyCsvReader](struct.LazyCsvReader.html)

[Source](../../src/polars_lazy/scan/csv.rs.html#32)

### impl [LazyCsvReader](struct.LazyCsvReader.html)

## [Source](../../src/polars_lazy/scan/csv.rs.html#34-37)
#### pub fn [map_parse_options](#method.map_parse_options)<F>(self, map_func: F) -> [LazyCsvReader](struct.LazyCsvReader.html)

[Source](../../src/polars_lazy/scan/csv.rs.html#34-37)

#### pub fn [map_parse_options](#method.map_parse_options)<F>(self, map_func: F) -> [LazyCsvReader](struct.LazyCsvReader.html)

Re-export to shorten code.

[Source](../../src/polars_lazy/scan/csv.rs.html#42)

#### pub fn [new_paths](#method.new_paths)(paths: Buffer<[PlRefPath](struct.PlRefPath.html)>) -> [LazyCsvReader](struct.LazyCsvReader.html)

[Source](../../src/polars_lazy/scan/csv.rs.html#46)

#### pub fn [new_with_sources](#method.new_with_sources)(sources: [ScanSources](enum.ScanSources.html)) -> [LazyCsvReader](struct.LazyCsvReader.html)

[Source](../../src/polars_lazy/scan/csv.rs.html#58)

#### pub fn [new](#method.new)(path: [PlRefPath](struct.PlRefPath.html)) -> [LazyCsvReader](struct.LazyCsvReader.html)

## [Source](../../src/polars_lazy/scan/csv.rs.html#64)
#### pub fn [with_skip_rows_after_header](#method.with_skip_rows_after_header)(self, offset: [usize](https://doc.rust-lang.org/nightly/std/primitive.usize.html)) -> [LazyCsvReader](struct.LazyCsvReader.html)

[Source](../../src/polars_lazy/scan/csv.rs.html#64)

#### pub fn [with_skip_rows_after_header](#method.with_skip_rows_after_header)(self, offset: [usize](https://doc.rust-lang.org/nightly/std/primitive.usize.html)) -> [LazyCsvReader](struct.LazyCsvReader.html)

Skip this number of rows after the header location.

## [Source](../../src/polars_lazy/scan/csv.rs.html#71)
#### pub fn [with_row_index](#method.with_row_index)(self, row_index: [Option](https://doc.rust-lang.org/nightly/core/option/enum.Option.html)<[RowIndex](struct.RowIndex.html)>) -> [LazyCsvReader](struct.LazyCsvReader.html)

[Source](../../src/polars_lazy/scan/csv.rs.html#71)

#### pub fn [with_row_index](#method.with_row_index)(self, row_index: [Option](https://doc.rust-lang.org/nightly/core/option/enum.Option.html)<[RowIndex](struct.RowIndex.html)>) -> [LazyCsvReader](struct.LazyCsvReader.html)

Add a row index column.

## [Source](../../src/polars_lazy/scan/csv.rs.html#79)
#### pub fn [with_n_rows](#method.with_n_rows)(self, num_rows: [Option](https://doc.rust-lang.org/nightly/core/option/enum.Option.html)<[usize](https://doc.rust-lang.org/nightly/std/primitive.usize.html)>) -> [LazyCsvReader](struct.LazyCsvReader.html)

[Source](../../src/polars_lazy/scan/csv.rs.html#79)

#### pub fn [with_n_rows](#method.with_n_rows)(self, num_rows: [Option](https://doc.rust-lang.org/nightly/core/option/enum.Option.html)<[usize](https://doc.rust-lang.org/nightly/std/primitive.usize.html)>) -> [LazyCsvReader](struct.LazyCsvReader.html)

Try to stop parsing when `n` rows are parsed. During multithreaded parsing the upper bound `n` cannot
be guaranteed.

## [Source](../../src/polars_lazy/scan/csv.rs.html#86)
#### pub fn [with_n_threads](#method.with_n_threads)(self, n_threads: [Option](https://doc.rust-lang.org/nightly/core/option/enum.Option.html)<[usize](https://doc.rust-lang.org/nightly/std/primitive.usize.html)>) -> [LazyCsvReader](struct.LazyCsvReader.html)

[Source](../../src/polars_lazy/scan/csv.rs.html#86)

#### pub fn [with_n_threads](#method.with_n_threads)(self, n_threads: [Option](https://doc.rust-lang.org/nightly/core/option/enum.Option.html)<[usize](https://doc.rust-lang.org/nightly/std/primitive.usize.html)>) -> [LazyCsvReader](struct.LazyCsvReader.html)

Sets the number of threads used for CSV parsing.

## [Source](../../src/polars_lazy/scan/csv.rs.html#95)
#### pub fn [with_infer_schema_length](#method.with_infer_schema_length)(self, num_rows: [Option](https://doc.rust-lang.org/nightly/core/option/enum.Option.html)<[usize](https://doc.rust-lang.org/nightly/std/primitive.usize.html)>) -> [LazyCsvReader](struct.LazyCsvReader.html)

[Source](../../src/polars_lazy/scan/csv.rs.html#95)

#### pub fn [with_infer_schema_length](#method.with_infer_schema_length)(self, num_rows: [Option](https://doc.rust-lang.org/nightly/core/option/enum.Option.html)<[usize](https://doc.rust-lang.org/nightly/std/primitive.usize.html)>) -> [LazyCsvReader](struct.LazyCsvReader.html)

Set the number of rows to use when inferring the csv schema.
The default is 100 rows.
Setting to [None](https://doc.rust-lang.org/nightly/core/option/enum.Option.html#variant.None) will do a full table scan, which is very slow.

[Source](../../src/polars_lazy/scan/csv.rs.html#101)

#### pub fn [with_infer_schema_files](#method.with_infer_schema_files)(
    self,
    infer_schema_files: [NonZero](https://doc.rust-lang.org/nightly/core/num/nonzero/struct.NonZero.html)<[usize](https://doc.rust-lang.org/nightly/std/primitive.usize.html)>,
) -> [LazyCsvReader](struct.LazyCsvReader.html)

## [Source](../../src/polars_lazy/scan/csv.rs.html#108)
#### pub fn [with_ignore_errors](#method.with_ignore_errors)(self, ignore: [bool](https://doc.rust-lang.org/nightly/std/primitive.bool.html)) -> [LazyCsvReader](struct.LazyCsvReader.html)

[Source](../../src/polars_lazy/scan/csv.rs.html#108)

#### pub fn [with_ignore_errors](#method.with_ignore_errors)(self, ignore: [bool](https://doc.rust-lang.org/nightly/std/primitive.bool.html)) -> [LazyCsvReader](struct.LazyCsvReader.html)

Continue with next batch when a ParserError is encountered.

## [Source](../../src/polars_lazy/scan/csv.rs.html#115)
#### pub fn [with_schema](#method.with_schema)(
    self,
    schema: [Option](https://doc.rust-lang.org/nightly/core/option/enum.Option.html)<[Arc](struct.Arc.html)<Schema<[DataType](enum.DataType.html), [()](https://doc.rust-lang.org/nightly/std/primitive.unit.html)>>>,
) -> [LazyCsvReader](struct.LazyCsvReader.html)

[Source](../../src/polars_lazy/scan/csv.rs.html#115)

#### pub fn [with_schema](#method.with_schema)(
    self,
    schema: [Option](https://doc.rust-lang.org/nightly/core/option/enum.Option.html)<[Arc](struct.Arc.html)<Schema<[DataType](enum.DataType.html), [()](https://doc.rust-lang.org/nightly/std/primitive.unit.html)>>>,
) -> [LazyCsvReader](struct.LazyCsvReader.html)

Set the CSV file’s schema

## [Source](../../src/polars_lazy/scan/csv.rs.html#123)
#### pub fn [with_skip_rows](#method.with_skip_rows)(self, skip_rows: [usize](https://doc.rust-lang.org/nightly/std/primitive.usize.html)) -> [LazyCsvReader](struct.LazyCsvReader.html)

[Source](../../src/polars_lazy/scan/csv.rs.html#123)

#### pub fn [with_skip_rows](#method.with_skip_rows)(self, skip_rows: [usize](https://doc.rust-lang.org/nightly/std/primitive.usize.html)) -> [LazyCsvReader](struct.LazyCsvReader.html)

Skip the first `n` rows during parsing. The header will be parsed at row `n`.
Note that by row we mean valid CSV, encoding and comments are respected.

## [Source](../../src/polars_lazy/scan/csv.rs.html#131)
#### pub fn [with_skip_lines](#method.with_skip_lines)(self, skip_lines: [usize](https://doc.rust-lang.org/nightly/std/primitive.usize.html)) -> [LazyCsvReader](struct.LazyCsvReader.html)

[Source](../../src/polars_lazy/scan/csv.rs.html#131)

#### pub fn [with_skip_lines](#method.with_skip_lines)(self, skip_lines: [usize](https://doc.rust-lang.org/nightly/std/primitive.usize.html)) -> [LazyCsvReader](struct.LazyCsvReader.html)

Skip the first `n` lines during parsing. The header will be parsed at line `n`.
We don’t respect CSV escaping when skipping lines.

[Source](../../src/polars_lazy/scan/csv.rs.html#137-140)

#### pub fn [with_column_names_overwrite](#method.with_column_names_overwrite)(
    self,
    column_names_overwrite: Buffer<[PlSmallStr](struct.PlSmallStr.html)>,
) -> [LazyCsvReader](struct.LazyCsvReader.html)

## [Source](../../src/polars_lazy/scan/csv.rs.html#148)
#### pub fn [with_dtype_overwrite](#method.with_dtype_overwrite)(
    self,
    schema: [Option](https://doc.rust-lang.org/nightly/core/option/enum.Option.html)<[Arc](struct.Arc.html)<Schema<[DataType](enum.DataType.html), [()](https://doc.rust-lang.org/nightly/std/primitive.unit.html)>>>,
) -> [LazyCsvReader](struct.LazyCsvReader.html)

[Source](../../src/polars_lazy/scan/csv.rs.html#148)

#### pub fn [with_dtype_overwrite](#method.with_dtype_overwrite)(
    self,
    schema: [Option](https://doc.rust-lang.org/nightly/core/option/enum.Option.html)<[Arc](struct.Arc.html)<Schema<[DataType](enum.DataType.html), [()](https://doc.rust-lang.org/nightly/std/primitive.unit.html)>>>,
) -> [LazyCsvReader](struct.LazyCsvReader.html)

Overwrite the schema with the dtypes in this given Schema. The given schema may be a subset of the total schema.

## [Source](../../src/polars_lazy/scan/csv.rs.html#155)
#### pub fn [with_dtype_overwrite_by_position](#method.with_dtype_overwrite_by_position)(
    self,
    dtypes: [Option](https://doc.rust-lang.org/nightly/core/option/enum.Option.html)<[Arc](struct.Arc.html)<[Vec](https://doc.rust-lang.org/nightly/alloc/vec/struct.Vec.html)<[DataType](enum.DataType.html)>>>,
) -> [LazyCsvReader](struct.LazyCsvReader.html)

[Source](../../src/polars_lazy/scan/csv.rs.html#155)

#### pub fn [with_dtype_overwrite_by_position](#method.with_dtype_overwrite_by_position)(
    self,
    dtypes: [Option](https://doc.rust-lang.org/nightly/core/option/enum.Option.html)<[Arc](struct.Arc.html)<[Vec](https://doc.rust-lang.org/nightly/alloc/vec/struct.Vec.html)<[DataType](enum.DataType.html)>>>,
) -> [LazyCsvReader](struct.LazyCsvReader.html)

Overwrite dtypes by position.

## [Source](../../src/polars_lazy/scan/csv.rs.html#162)
#### pub fn [with_has_header](#method.with_has_header)(self, has_header: [bool](https://doc.rust-lang.org/nightly/std/primitive.bool.html)) -> [LazyCsvReader](struct.LazyCsvReader.html)

[Source](../../src/polars_lazy/scan/csv.rs.html#162)

#### pub fn [with_has_header](#method.with_has_header)(self, has_header: [bool](https://doc.rust-lang.org/nightly/std/primitive.bool.html)) -> [LazyCsvReader](struct.LazyCsvReader.html)

Set whether the CSV file has headers

## [Source](../../src/polars_lazy/scan/csv.rs.html#169)
#### pub fn [with_chunk_size](#method.with_chunk_size)(self, chunk_size: [usize](https://doc.rust-lang.org/nightly/std/primitive.usize.html)) -> [LazyCsvReader](struct.LazyCsvReader.html)

[Source](../../src/polars_lazy/scan/csv.rs.html#169)

#### pub fn [with_chunk_size](#method.with_chunk_size)(self, chunk_size: [usize](https://doc.rust-lang.org/nightly/std/primitive.usize.html)) -> [LazyCsvReader](struct.LazyCsvReader.html)

Sets the chunk size used by the parser. This influences performance. This can be used as a way to reduce memory usage during the parsing at the cost of performance.

## [Source](../../src/polars_lazy/scan/csv.rs.html#176)
#### pub fn [with_separator](#method.with_separator)(self, separator: [u8](https://doc.rust-lang.org/nightly/std/primitive.u8.html)) -> [LazyCsvReader](struct.LazyCsvReader.html)

[Source](../../src/polars_lazy/scan/csv.rs.html#176)

#### pub fn [with_separator](#method.with_separator)(self, separator: [u8](https://doc.rust-lang.org/nightly/std/primitive.u8.html)) -> [LazyCsvReader](struct.LazyCsvReader.html)

Set the CSV file’s column separator as a byte character

## [Source](../../src/polars_lazy/scan/csv.rs.html#182)
#### pub fn [with_comment_prefix](#method.with_comment_prefix)(
    self,
    comment_prefix: [Option](https://doc.rust-lang.org/nightly/core/option/enum.Option.html)<[PlSmallStr](struct.PlSmallStr.html)>,
) -> [LazyCsvReader](struct.LazyCsvReader.html)

[Source](../../src/polars_lazy/scan/csv.rs.html#182)

#### pub fn [with_comment_prefix](#method.with_comment_prefix)(
    self,
    comment_prefix: [Option](https://doc.rust-lang.org/nightly/core/option/enum.Option.html)<[PlSmallStr](struct.PlSmallStr.html)>,
) -> [LazyCsvReader](struct.LazyCsvReader.html)

Set the comment prefix for this instance. Lines starting with this prefix will be ignored.

## [Source](../../src/polars_lazy/scan/csv.rs.html#196)
#### pub fn [with_quote_char](#method.with_quote_char)(self, quote_char: [Option](https://doc.rust-lang.org/nightly/core/option/enum.Option.html)<[u8](https://doc.rust-lang.org/nightly/std/primitive.u8.html)>) -> [LazyCsvReader](struct.LazyCsvReader.html)

[Source](../../src/polars_lazy/scan/csv.rs.html#196)

#### pub fn [with_quote_char](#method.with_quote_char)(self, quote_char: [Option](https://doc.rust-lang.org/nightly/core/option/enum.Option.html)<[u8](https://doc.rust-lang.org/nightly/std/primitive.u8.html)>) -> [LazyCsvReader](struct.LazyCsvReader.html)

Set the `char` used as quote char. The default is `b'"'`. If set to [`None`](https://doc.rust-lang.org/nightly/core/option/enum.Option.html#variant.None) quoting is disabled.

## [Source](../../src/polars_lazy/scan/csv.rs.html#202)
#### pub fn [with_eol_char](#method.with_eol_char)(self, eol_char: [u8](https://doc.rust-lang.org/nightly/std/primitive.u8.html)) -> [LazyCsvReader](struct.LazyCsvReader.html)

[Source](../../src/polars_lazy/scan/csv.rs.html#202)

#### pub fn [with_eol_char](#method.with_eol_char)(self, eol_char: [u8](https://doc.rust-lang.org/nightly/std/primitive.u8.html)) -> [LazyCsvReader](struct.LazyCsvReader.html)

Set the `char` used as end of line. The default is `b'\n'`.

## [Source](../../src/polars_lazy/scan/csv.rs.html#208)
#### pub fn [with_null_values](#method.with_null_values)(self, null_values: [Option](https://doc.rust-lang.org/nightly/core/option/enum.Option.html)<[NullValues](enum.NullValues.html)>) -> [LazyCsvReader](struct.LazyCsvReader.html)

[Source](../../src/polars_lazy/scan/csv.rs.html#208)

#### pub fn [with_null_values](#method.with_null_values)(self, null_values: [Option](https://doc.rust-lang.org/nightly/core/option/enum.Option.html)<[NullValues](enum.NullValues.html)>) -> [LazyCsvReader](struct.LazyCsvReader.html)

Set values that will be interpreted as missing/ null.

## [Source](../../src/polars_lazy/scan/csv.rs.html#213)
#### pub fn [with_missing_is_null](#method.with_missing_is_null)(self, missing_is_null: [bool](https://doc.rust-lang.org/nightly/std/primitive.bool.html)) -> [LazyCsvReader](struct.LazyCsvReader.html)

[Source](../../src/polars_lazy/scan/csv.rs.html#213)

#### pub fn [with_missing_is_null](#method.with_missing_is_null)(self, missing_is_null: [bool](https://doc.rust-lang.org/nightly/std/primitive.bool.html)) -> [LazyCsvReader](struct.LazyCsvReader.html)

Treat missing fields as null.

## [Source](../../src/polars_lazy/scan/csv.rs.html#219)
#### pub fn [with_cache](#method.with_cache)(self, cache: [bool](https://doc.rust-lang.org/nightly/std/primitive.bool.html)) -> [LazyCsvReader](struct.LazyCsvReader.html)

[Source](../../src/polars_lazy/scan/csv.rs.html#219)

#### pub fn [with_cache](#method.with_cache)(self, cache: [bool](https://doc.rust-lang.org/nightly/std/primitive.bool.html)) -> [LazyCsvReader](struct.LazyCsvReader.html)

Cache the DataFrame after reading.

## [Source](../../src/polars_lazy/scan/csv.rs.html#226)
#### pub fn [with_low_memory](#method.with_low_memory)(self, low_memory: [bool](https://doc.rust-lang.org/nightly/std/primitive.bool.html)) -> [LazyCsvReader](struct.LazyCsvReader.html)

[Source](../../src/polars_lazy/scan/csv.rs.html#226)

#### pub fn [with_low_memory](#method.with_low_memory)(self, low_memory: [bool](https://doc.rust-lang.org/nightly/std/primitive.bool.html)) -> [LazyCsvReader](struct.LazyCsvReader.html)

Reduce memory usage at the expense of performance

## [Source](../../src/polars_lazy/scan/csv.rs.html#233)
#### pub fn [with_encoding](#method.with_encoding)(self, encoding: [CsvEncoding](enum.CsvEncoding.html)) -> [LazyCsvReader](struct.LazyCsvReader.html)

[Source](../../src/polars_lazy/scan/csv.rs.html#233)

#### pub fn [with_encoding](#method.with_encoding)(self, encoding: [CsvEncoding](enum.CsvEncoding.html)) -> [LazyCsvReader](struct.LazyCsvReader.html)

Set  `CsvEncoding`

## [Source](../../src/polars_lazy/scan/csv.rs.html#240)
#### pub fn [with_try_parse_dates](#method.with_try_parse_dates)(self, try_parse_dates: [bool](https://doc.rust-lang.org/nightly/std/primitive.bool.html)) -> [LazyCsvReader](struct.LazyCsvReader.html)

**crate feature `temporal`** only.

[Source](../../src/polars_lazy/scan/csv.rs.html#240)

#### pub fn [with_try_parse_dates](#method.with_try_parse_dates)(self, try_parse_dates: [bool](https://doc.rust-lang.org/nightly/std/primitive.bool.html)) -> [LazyCsvReader](struct.LazyCsvReader.html)

**crate feature**only.

`temporal`
Automatically try to parse dates/datetimes and time.
If parsing fails, columns remain of dtype [`DataType::String`](enum.DataType.html#variant.String).

## [Source](../../src/polars_lazy/scan/csv.rs.html#246)
#### pub fn [with_raise_if_empty](#method.with_raise_if_empty)(self, raise_if_empty: [bool](https://doc.rust-lang.org/nightly/std/primitive.bool.html)) -> [LazyCsvReader](struct.LazyCsvReader.html)

[Source](../../src/polars_lazy/scan/csv.rs.html#246)

#### pub fn [with_raise_if_empty](#method.with_raise_if_empty)(self, raise_if_empty: [bool](https://doc.rust-lang.org/nightly/std/primitive.bool.html)) -> [LazyCsvReader](struct.LazyCsvReader.html)

Raise an error if CSV is empty (otherwise return an empty frame)

## [Source](../../src/polars_lazy/scan/csv.rs.html#253)
#### pub fn [with_truncate_ragged_lines](#method.with_truncate_ragged_lines)(
    self,
    truncate_ragged_lines: [bool](https://doc.rust-lang.org/nightly/std/primitive.bool.html),
) -> [LazyCsvReader](struct.LazyCsvReader.html)

[Source](../../src/polars_lazy/scan/csv.rs.html#253)

#### pub fn [with_truncate_ragged_lines](#method.with_truncate_ragged_lines)(
    self,
    truncate_ragged_lines: [bool](https://doc.rust-lang.org/nightly/std/primitive.bool.html),
) -> [LazyCsvReader](struct.LazyCsvReader.html)

Truncate lines that are longer than the schema.

[Source](../../src/polars_lazy/scan/csv.rs.html#258)

#### pub fn [with_decimal_comma](#method.with_decimal_comma)(self, decimal_comma: [bool](https://doc.rust-lang.org/nightly/std/primitive.bool.html)) -> [LazyCsvReader](struct.LazyCsvReader.html)

## [Source](../../src/polars_lazy/scan/csv.rs.html#264)
#### pub fn [with_glob](#method.with_glob)(self, toggle: [bool](https://doc.rust-lang.org/nightly/std/primitive.bool.html)) -> [LazyCsvReader](struct.LazyCsvReader.html)

[Source](../../src/polars_lazy/scan/csv.rs.html#264)

#### pub fn [with_glob](#method.with_glob)(self, toggle: [bool](https://doc.rust-lang.org/nightly/std/primitive.bool.html)) -> [LazyCsvReader](struct.LazyCsvReader.html)

Expand path given via globbing rules.

[Source](../../src/polars_lazy/scan/csv.rs.html#269)

#### pub fn [with_cloud_options](#method.with_cloud_options)(
    self,
    cloud_options: [Option](https://doc.rust-lang.org/nightly/core/option/enum.Option.html)<[CloudOptions](cloud/struct.CloudOptions.html)>,
) -> [LazyCsvReader](struct.LazyCsvReader.html)

## [Source](../../src/polars_lazy/scan/csv.rs.html#277-279)
#### pub fn [with_schema_modify](#method.with_schema_modify)<F>(self, f: F) -> [Result](https://doc.rust-lang.org/nightly/core/result/enum.Result.html)<[LazyCsvReader](struct.LazyCsvReader.html), [PolarsError](enum.PolarsError.html)>

[Source](../../src/polars_lazy/scan/csv.rs.html#277-279)

#### pub fn [with_schema_modify](#method.with_schema_modify)<F>(self, f: F) -> [Result](https://doc.rust-lang.org/nightly/core/result/enum.Result.html)<[LazyCsvReader](struct.LazyCsvReader.html), [PolarsError](enum.PolarsError.html)>

Modify a schema before we run the lazy scanning.

Important! Run this function latest in the builder!

[Source](../../src/polars_lazy/scan/csv.rs.html#360)

#### pub fn [with_include_file_paths](#method.with_include_file_paths)(
    self,
    include_file_paths: [Option](https://doc.rust-lang.org/nightly/core/option/enum.Option.html)<[PlSmallStr](struct.PlSmallStr.html)>,
) -> [LazyCsvReader](struct.LazyCsvReader.html)

[Source](../../src/polars_lazy/scan/csv.rs.html#366)

#### pub fn [with_missing_columns_policy](#method.with_missing_columns_policy)(
    self,
    policy: [Option](https://doc.rust-lang.org/nightly/core/option/enum.Option.html)<[MissingColumnsPolicy](enum.MissingColumnsPolicy.html)>,
) -> [LazyCsvReader](struct.LazyCsvReader.html)

## Trait Implementations

## [Source](../../src/polars_lazy/scan/csv.rs.html#19)
### impl [Clone](https://doc.rust-lang.org/nightly/core/clone/trait.Clone.html) for [LazyCsvReader](struct.LazyCsvReader.html)

[Source](../../src/polars_lazy/scan/csv.rs.html#19)

### impl [Clone](https://doc.rust-lang.org/nightly/core/clone/trait.Clone.html) for [LazyCsvReader](struct.LazyCsvReader.html)

## [Source](../../src/polars_lazy/scan/csv.rs.html#19)
#### fn [clone](https://doc.rust-lang.org/nightly/core/clone/trait.Clone.html#tymethod.clone)(&self) -> [LazyCsvReader](struct.LazyCsvReader.html)

[Source](../../src/polars_lazy/scan/csv.rs.html#19)

#### fn [clone](https://doc.rust-lang.org/nightly/core/clone/trait.Clone.html#tymethod.clone)(&self) -> [LazyCsvReader](struct.LazyCsvReader.html)

[Read more](https://doc.rust-lang.org/nightly/core/clone/trait.Clone.html#tymethod.clone)

## 1.0.0 · [Source](https://doc.rust-lang.org/nightly/src/core/clone.rs.html#245-247)
#### fn [clone_from](https://doc.rust-lang.org/nightly/core/clone/trait.Clone.html#method.clone_from)(&mut self, source: &Self)

 

[Source](https://doc.rust-lang.org/nightly/src/core/clone.rs.html#245-247)

#### fn [clone_from](https://doc.rust-lang.org/nightly/core/clone/trait.Clone.html#method.clone_from)(&mut self, source: &Self)

`source`. [Read more](https://doc.rust-lang.org/nightly/core/clone/trait.Clone.html#method.clone_from)

## [Source](../../src/polars_lazy/scan/csv.rs.html#372)
### impl [LazyFileListReader](trait.LazyFileListReader.html) for [LazyCsvReader](struct.LazyCsvReader.html)

[Source](../../src/polars_lazy/scan/csv.rs.html#372)

### impl [LazyFileListReader](trait.LazyFileListReader.html) for [LazyCsvReader](struct.LazyCsvReader.html)

## [Source](../../src/polars_lazy/scan/csv.rs.html#443)
#### fn [with_rechunk](trait.LazyFileListReader.html#tymethod.with_rechunk)(self, rechunk: [bool](https://doc.rust-lang.org/nightly/std/primitive.bool.html)) -> [LazyCsvReader](struct.LazyCsvReader.html)

[Source](../../src/polars_lazy/scan/csv.rs.html#443)

#### fn [with_rechunk](trait.LazyFileListReader.html#tymethod.with_rechunk)(self, rechunk: [bool](https://doc.rust-lang.org/nightly/std/primitive.bool.html)) -> [LazyCsvReader](struct.LazyCsvReader.html)

Rechunk the memory to contiguous chunks when parsing is done.

## [Source](../../src/polars_lazy/scan/csv.rs.html#450)
#### fn [n_rows](trait.LazyFileListReader.html#tymethod.n_rows)(&self) -> [Option](https://doc.rust-lang.org/nightly/core/option/enum.Option.html)<[usize](https://doc.rust-lang.org/nightly/std/primitive.usize.html)>

[Source](../../src/polars_lazy/scan/csv.rs.html#450)

#### fn [n_rows](trait.LazyFileListReader.html#tymethod.n_rows)(&self) -> [Option](https://doc.rust-lang.org/nightly/core/option/enum.Option.html)<[usize](https://doc.rust-lang.org/nightly/std/primitive.usize.html)>

Try to stop parsing when `n` rows are parsed. During multithreaded parsing the upper bound `n` cannot
be guaranteed.

## [Source](../../src/polars_lazy/scan/csv.rs.html#472)
#### fn [cloud_options](trait.LazyFileListReader.html#method.cloud_options)(&self) -> [Option](https://doc.rust-lang.org/nightly/core/option/enum.Option.html)<&[CloudOptions](cloud/struct.CloudOptions.html)>

[Source](../../src/polars_lazy/scan/csv.rs.html#472)

#### fn [cloud_options](trait.LazyFileListReader.html#method.cloud_options)(&self) -> [Option](https://doc.rust-lang.org/nightly/core/option/enum.Option.html)<&[CloudOptions](cloud/struct.CloudOptions.html)>

[CloudOptions](cloud/struct.CloudOptions.html) used to list files.

## [Source](../../src/polars_lazy/scan/csv.rs.html#411)
#### fn [finish_no_glob](trait.LazyFileListReader.html#tymethod.finish_no_glob)(self) -> [Result](https://doc.rust-lang.org/nightly/core/result/enum.Result.html)<[LazyFrame](struct.LazyFrame.html), [PolarsError](enum.PolarsError.html)>

[Source](../../src/polars_lazy/scan/csv.rs.html#411)

#### fn [finish_no_glob](trait.LazyFileListReader.html#tymethod.finish_no_glob)(self) -> [Result](https://doc.rust-lang.org/nightly/core/result/enum.Result.html)<[LazyFrame](struct.LazyFrame.html), [PolarsError](enum.PolarsError.html)>

[Source](../../src/polars_lazy/scan/csv.rs.html#415)

#### fn [glob](trait.LazyFileListReader.html#method.glob)(&self) -> [bool](https://doc.rust-lang.org/nightly/std/primitive.bool.html)

## [Source](../../src/polars_lazy/scan/csv.rs.html#419)
#### fn [sources](trait.LazyFileListReader.html#tymethod.sources)(&self) -> &[ScanSources](enum.ScanSources.html)

[Source](../../src/polars_lazy/scan/csv.rs.html#419)

#### fn [sources](trait.LazyFileListReader.html#tymethod.sources)(&self) -> &[ScanSources](enum.ScanSources.html)

## [Source](../../src/polars_lazy/scan/csv.rs.html#423)
#### fn [with_sources](trait.LazyFileListReader.html#tymethod.with_sources)(self, sources: [ScanSources](enum.ScanSources.html)) -> [LazyCsvReader](struct.LazyCsvReader.html)

[Source](../../src/polars_lazy/scan/csv.rs.html#423)

#### fn [with_sources](trait.LazyFileListReader.html#tymethod.with_sources)(self, sources: [ScanSources](enum.ScanSources.html)) -> [LazyCsvReader](struct.LazyCsvReader.html)

## [Source](../../src/polars_lazy/scan/csv.rs.html#428)
#### fn [with_n_rows](trait.LazyFileListReader.html#tymethod.with_n_rows)(self, n_rows: impl [Into](https://doc.rust-lang.org/nightly/core/convert/trait.Into.html)<[Option](https://doc.rust-lang.org/nightly/core/option/enum.Option.html)<[usize](https://doc.rust-lang.org/nightly/std/primitive.usize.html)>>) -> [LazyCsvReader](struct.LazyCsvReader.html)

[Source](../../src/polars_lazy/scan/csv.rs.html#428)

#### fn [with_n_rows](trait.LazyFileListReader.html#tymethod.with_n_rows)(self, n_rows: impl [Into](https://doc.rust-lang.org/nightly/core/convert/trait.Into.html)<[Option](https://doc.rust-lang.org/nightly/core/option/enum.Option.html)<[usize](https://doc.rust-lang.org/nightly/std/primitive.usize.html)>>) -> [LazyCsvReader](struct.LazyCsvReader.html)

## [Source](../../src/polars_lazy/scan/csv.rs.html#433)
#### fn [with_row_index](trait.LazyFileListReader.html#tymethod.with_row_index)(self, row_index: impl [Into](https://doc.rust-lang.org/nightly/core/convert/trait.Into.html)<[Option](https://doc.rust-lang.org/nightly/core/option/enum.Option.html)<[RowIndex](struct.RowIndex.html)>>) -> [LazyCsvReader](struct.LazyCsvReader.html)

[Source](../../src/polars_lazy/scan/csv.rs.html#433)

#### fn [with_row_index](trait.LazyFileListReader.html#tymethod.with_row_index)(self, row_index: impl [Into](https://doc.rust-lang.org/nightly/core/convert/trait.Into.html)<[Option](https://doc.rust-lang.org/nightly/core/option/enum.Option.html)<[RowIndex](struct.RowIndex.html)>>) -> [LazyCsvReader](struct.LazyCsvReader.html)

## [Source](../../src/polars_lazy/scan/csv.rs.html#459)
#### fn [concat_impl](trait.LazyFileListReader.html#method.concat_impl)(&self, lfs: [Vec](https://doc.rust-lang.org/nightly/alloc/vec/struct.Vec.html)<[LazyFrame](struct.LazyFrame.html)>) -> [Result](https://doc.rust-lang.org/nightly/core/result/enum.Result.html)<[LazyFrame](struct.LazyFrame.html), [PolarsError](enum.PolarsError.html)>

[Source](../../src/polars_lazy/scan/csv.rs.html#459)

#### fn [concat_impl](trait.LazyFileListReader.html#method.concat_impl)(&self, lfs: [Vec](https://doc.rust-lang.org/nightly/alloc/vec/struct.Vec.html)<[LazyFrame](struct.LazyFrame.html)>) -> [Result](https://doc.rust-lang.org/nightly/core/result/enum.Result.html)<[LazyFrame](struct.LazyFrame.html), [PolarsError](enum.PolarsError.html)>

## [Source](../../src/polars_lazy/scan/file_list_reader.rs.html#94)
#### fn [with_paths](trait.LazyFileListReader.html#method.with_paths)(self, paths: Buffer<[PlRefPath](struct.PlRefPath.html)>) -> Self

[Source](../../src/polars_lazy/scan/file_list_reader.rs.html#94)

#### fn [with_paths](trait.LazyFileListReader.html#method.with_paths)(self, paths: Buffer<[PlRefPath](struct.PlRefPath.html)>) -> Self

## Auto Trait Implementations

### impl [Freeze](https://doc.rust-lang.org/nightly/core/marker/trait.Freeze.html) for [LazyCsvReader](struct.LazyCsvReader.html)

### impl ![RefUnwindSafe](https://doc.rust-lang.org/nightly/core/panic/unwind_safe/trait.RefUnwindSafe.html) for [LazyCsvReader](struct.LazyCsvReader.html)

### impl [Send](https://doc.rust-lang.org/nightly/core/marker/trait.Send.html) for [LazyCsvReader](struct.LazyCsvReader.html)

### impl [Sync](https://doc.rust-lang.org/nightly/core/marker/trait.Sync.html) for [LazyCsvReader](struct.LazyCsvReader.html)

### impl [Unpin](https://doc.rust-lang.org/nightly/core/marker/trait.Unpin.html) for [LazyCsvReader](struct.LazyCsvReader.html)

### impl [UnsafeUnpin](https://doc.rust-lang.org/nightly/core/marker/trait.UnsafeUnpin.html) for [LazyCsvReader](struct.LazyCsvReader.html)

### impl ![UnwindSafe](https://doc.rust-lang.org/nightly/core/panic/unwind_safe/trait.UnwindSafe.html) for [LazyCsvReader](struct.LazyCsvReader.html)

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

### impl<T> PlanCallbackArgs for T

### impl<T> PlanCallbackOut for T

### impl<T> Read<Exclusive, BecauseExclusive> for Twhere
    T: ?[Sized](https://doc.rust-lang.org/nightly/core/marker/trait.Sized.html),

[Sized](https://doc.rust-lang.org/nightly/core/marker/trait.Sized.html),

# Citations

1. Source page: https://docs.pola.rs/api/rust/dev/polars/prelude/struct.LazyCsvReader.html
