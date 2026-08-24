---
type: Web Page
title: polars::frame - Rust
description: DataFrame module.
resource: https://docs.pola.rs/api/rust/dev/polars/frame/index.html
timestamp: '2026-08-24T07:00:57.822031+00:00'
---

## Expand description

DataFrame module.

## Modules

## Structs

- [Data](struct.DataFrame.html)Frame
- A contiguous growable collection of [`Column`](../prelude/enum.Column.html) s that have the same length.
- [Phys](struct.PhysRecordBatchIter.html)Record Batch Iter
- [Record](struct.RecordBatchIter.html)Batch Iter

## Enums

- [Pivot](enum.PivotColumnNaming.html)Column Naming
- Naming strategy for the results of a pivot.
- [Record](enum.RecordBatchIterWrap.html)Batch Iter Wrap
- [Unique](enum.UniqueKeepStrategy.html)Keep Strategy

## Functions

- [chunk_](fn.chunk_df_for_writing.html)df_ for_ writing
- Split DataFrame into chunks in preparation for writing. The chunks have a maximum number of rows per chunk to ensure reasonable memory efficiency when reading the resulting file, and a minimum size per chunk to ensure reasonable performance when writing.

# Citations

1. Source page: https://docs.pola.rs/api/rust/dev/polars/frame/index.html
