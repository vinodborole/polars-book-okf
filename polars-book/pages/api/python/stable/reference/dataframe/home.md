---
type: Web Page
title: DataFrame — Polars  documentation
resource: https://docs.pola.rs/api/python/stable/reference/dataframe/index.html
timestamp: '2026-08-03T09:49:29.273788+00:00'
---

# DataFrame

This page gives an overview of all public DataFrame methods.

- 
*class* polars.DataFrame(
- *data: FrameInitTypes | [None](https://docs.python.org/3/library/constants.html#None) = None* ,
- *schema: SchemaDefinition | [None](https://docs.python.org/3/library/constants.html#None) = None* ,
- *** ,
- *schema_overrides: SchemaDict | [None](https://docs.python.org/3/library/constants.html#None) = None* ,
- *strict: [bool](https://docs.python.org/3/library/functions.html#bool) = True* ,
- *orient: Orientation | [None](https://docs.python.org/3/library/constants.html#None) = None* ,
- *infer_schema_length: [int](https://docs.python.org/3/library/functions.html#int) | [None](https://docs.python.org/3/library/constants.html#None) = 100* ,
- *nan_to_null: [bool](https://docs.python.org/3/library/functions.html#bool) = False* ,
- *height: [int](https://docs.python.org/3/library/functions.html#int) | [None](https://docs.python.org/3/library/constants.html#None) = None* ,
- Two-dimensional data structure representing data as a table with rows and columns. 
  - Parameters:
    - **data** dict, Sequence, ndarray, Series, or pandas.DataFrame
    - Two-dimensional data in various forms; dict input must contain Sequences, Generators, or a `range` . Sequence may contain Series or other Sequences.
    - **schema** Sequence of str, (str,DataType) pairs, or a {str:DataType,} dict
    - The schema of the resulting DataFrame. The schema may be declared in several ways: 
      - As a dict of {name:type} pairs; if type is None, it will be auto-inferred.
      - As a list of column names; in this case types are automatically inferred.
      - As a list of (name,type) pairs; this is equivalent to the dictionary form.
 The order of the schema determines the column order of the frame. When passing a dict, its insertion order is respected. To override specific column data types by name without changing column order, use `schema_overrides` instead.If you supply a list of column names that does not match the names in the underlying data, the names given here will overwrite them. The number of names given in the schema should match the underlying data dimensions. If set to `None` (default), the schema is inferred from the data.
    - **schema_overrides** dict, default None
    - Support type specification or override of one or more columns; note that any dtypes inferred from the schema param will be overridden. The number of entries in the schema should match the underlying data dimensions, unless a sequence of dictionaries is being passed, in which case a *partial* schema can be declared to prevent specific fields from being loaded.
    - **strict** bool, default True
    - Throw an error if any `data` value does not exactly match the given or inferred
data type for that column. If set to`False` , values that do not match the data
type are cast to that data type or, if casting is not possible, set to null
instead.
    - **orient** {‘col’, ‘row’}, default None
    - Whether to interpret two-dimensional data as columns or as rows. If None, the orientation is inferred by matching the columns and data dimensions. If this does not yield conclusive results, column orientation is used.
    - **infer_schema_length** int or None
    - The maximum number of rows to scan for schema inference. If set to `None` , the
full data may be scanned*(this can be slow)* . This parameter only applies if
the input data is a sequence or generator of rows; other input is read as-is.
    - **nan_to_null** bool, default False
    - If the data comes from one or more numpy arrays, can optionally convert input data np.nan values to null instead. This is a no-op for all other input data.
    - **height** int or None, default None
    - Allows constructing DataFrames with 0 width and a specified height. If passed with data, ensures the resulting DataFrame has this height. Warning This functionality is considered **unstable** . It may be changed
at any point without it being considered a breaking change.
 Notes Polars explicitly does not support subclassing of its core data types. See the following GitHub issue for possible workarounds: [pola-rs/polars#2846](https://github.com/pola-rs/polars/issues/2846#issuecomment-1711799869)Examples Constructing a DataFrame from a dictionary: >>> data = {"a": [1, 2], "b": [3, 4]} >>> df = pl.DataFrame(data) >>> df shape: (2, 2) ┌─────┬─────┐ │ a ┆ b │ │ --- ┆ --- │ │ i64 ┆ i64 │ ╞═════╪═════╡ │ 1 ┆ 3 │ │ 2 ┆ 4 │ └─────┴─────┘ Notice that the dtypes are automatically inferred as polars Int64: >>> df.dtypes [Int64, Int64] To specify a more detailed/specific frame schema you can supply the `schema` parameter with a dictionary of (name,dtype) pairs…>>> data = {"col1": [0, 2], "col2": [3, 7]} >>> df2 = pl.DataFrame(data, schema={"col1": pl.Float32, "col2": pl.Int64}) >>> df2 shape: (2, 2) ┌──────┬──────┐ │ col1 ┆ col2 │ │ --- ┆ --- │ │ f32 ┆ i64 │ ╞══════╪══════╡ │ 0.0 ┆ 3 │ │ 2.0 ┆ 7 │ └──────┴──────┘ …a sequence of (name,dtype) pairs… >>> data = {"col1": [1, 2], "col2": [3, 4]} >>> df3 = pl.DataFrame(data, schema=[("col1", pl.Float32), ("col2", pl.Int64)]) >>> df3 shape: (2, 2) ┌──────┬──────┐ │ col1 ┆ col2 │ │ --- ┆ --- │ │ f32 ┆ i64 │ ╞══════╪══════╡ │ 1.0 ┆ 3 │ │ 2.0 ┆ 4 │ └──────┴──────┘ …or a list of typed Series. >>> data = [ ... pl.Series("col1", [1, 2], dtype=pl.Float32), ... pl.Series("col2", [3, 4], dtype=pl.Int64), ... ] >>> df4 = pl.DataFrame(data) >>> df4 shape: (2, 2) ┌──────┬──────┐ │ col1 ┆ col2 │ │ --- ┆ --- │ │ f32 ┆ i64 │ ╞══════╪══════╡ │ 1.0 ┆ 3 │ │ 2.0 ┆ 4 │ └──────┴──────┘ Constructing a DataFrame from a numpy ndarray, specifying column names: >>> import numpy as np >>> data = np.array([(1, 2), (3, 4)], dtype=np.int64) >>> df5 = pl.DataFrame(data, schema=["a", "b"], orient="col") >>> df5 shape: (2, 2) ┌─────┬─────┐ │ a ┆ b │ │ --- ┆ --- │ │ i64 ┆ i64 │ ╞═════╪═════╡ │ 1 ┆ 3 │ │ 2 ┆ 4 │ └─────┴─────┘ Constructing a DataFrame from a list of lists, row orientation specified: >>> data = [[1, 2, 3], [4, 5, 6]] >>> df6 = pl.DataFrame(data, schema=["a", "b", "c"], orient="row") >>> df6 shape: (2, 3) ┌─────┬─────┬─────┐ │ a ┆ b ┆ c │ │ --- ┆ --- ┆ --- │ │ i64 ┆ i64 ┆ i64 │ ╞═════╪═════╪═════╡ │ 1 ┆ 2 ┆ 3 │ │ 4 ┆ 5 ┆ 6 │ └─────┴─────┴─────┘ **Methods:**[`approx_n_unique`](api/polars.DataFrame.approx_n_unique.html#polars.DataFrame.approx_n_unique)Approximate count of unique values. [`bottom_k`](api/polars.DataFrame.bottom_k.html#polars.DataFrame.bottom_k)Return the `k` smallest rows.[`cast`](api/polars.DataFrame.cast.html#polars.DataFrame.cast)Cast DataFrame column(s) to the specified dtype(s). [`clear`](api/polars.DataFrame.clear.html#polars.DataFrame.clear)Create an empty (n=0) or `n` -row null-filled (n>0) copy of the DataFrame.[`clone`](api/polars.DataFrame.clone.html#polars.DataFrame.clone)Create a copy of this DataFrame. [`collect_schema`](api/polars.DataFrame.collect_schema.html#polars.DataFrame.collect_schema)Get an ordered mapping of column names to their data type. [`corr`](api/polars.DataFrame.corr.html#polars.DataFrame.corr)Return pairwise Pearson product-moment correlation coefficients between columns. [`count`](api/polars.DataFrame.count.html#polars.DataFrame.count)Return the number of non-null elements for each column. [`describe`](api/polars.DataFrame.describe.html#polars.DataFrame.describe)Summary statistics for a DataFrame. [`deserialize`](api/polars.DataFrame.deserialize.html#polars.DataFrame.deserialize)Read a serialized DataFrame from a file. [`drop`](api/polars.DataFrame.drop.html#polars.DataFrame.drop)Remove columns from the dataframe. [`drop_in_place`](api/polars.DataFrame.drop_in_place.html#polars.DataFrame.drop_in_place)Drop a single column in-place and return the dropped column. [`drop_nans`](api/polars.DataFrame.drop_nans.html#polars.DataFrame.drop_nans)Drop all rows that contain one or more NaN values. [`drop_nulls`](api/polars.DataFrame.drop_nulls.html#polars.DataFrame.drop_nulls)Drop all rows that contain one or more null values. [`equals`](api/polars.DataFrame.equals.html#polars.DataFrame.equals)Check whether the DataFrame is equal to another DataFrame. [`estimated_size`](api/polars.DataFrame.estimated_size.html#polars.DataFrame.estimated_size)Return an estimation of the total (heap) allocated size of the `DataFrame` .[`explode`](api/polars.DataFrame.explode.html#polars.DataFrame.explode)Explode the dataframe to long format by exploding the given columns. [`extend`](api/polars.DataFrame.extend.html#polars.DataFrame.extend)Extend the memory backed by this `DataFrame` with the values from`other` .[`fill_nan`](api/polars.DataFrame.fill_nan.html#polars.DataFrame.fill_nan)Fill floating point NaN values by an Expression evaluation. [`fill_null`](api/polars.DataFrame.fill_null.html#polars.DataFrame.fill_null)Fill null values using the specified value or strategy. [`filter`](api/polars.DataFrame.filter.html#polars.DataFrame.filter)Filter rows, retaining those that match the given predicate expression(s). [`fold`](api/polars.DataFrame.fold.html#polars.DataFrame.fold)Apply a horizontal reduction on a DataFrame. [`gather`](api/polars.DataFrame.gather.html#polars.DataFrame.gather)Selects rows from this DataFrame at the given indices. [`gather_every`](api/polars.DataFrame.gather_every.html#polars.DataFrame.gather_every)Take every nth row in the DataFrame and return as a new DataFrame. [`get_column`](api/polars.DataFrame.get_column.html#polars.DataFrame.get_column)Get a single column by name. [`get_column_index`](api/polars.DataFrame.get_column_index.html#polars.DataFrame.get_column_index)Find the index of a column by name. [`get_columns`](api/polars.DataFrame.get_columns.html#polars.DataFrame.get_columns)Get the DataFrame as a List of Series. [`glimpse`](api/polars.DataFrame.glimpse.html#polars.DataFrame.glimpse)Return a dense preview of the DataFrame. [`group_by`](api/polars.DataFrame.group_by.html#polars.DataFrame.group_by)Start a group by operation. [`group_by_dynamic`](api/polars.DataFrame.group_by_dynamic.html#polars.DataFrame.group_by_dynamic)Group based on a time value (or index value of type Int32, Int64). [`hash_rows`](api/polars.DataFrame.hash_rows.html#polars.DataFrame.hash_rows)Hash and combine the rows in this DataFrame. [`head`](api/polars.DataFrame.head.html#polars.DataFrame.head)Get the first `n` rows.[`hstack`](api/polars.DataFrame.hstack.html#polars.DataFrame.hstack)Return a new DataFrame grown horizontally by stacking multiple Series to it. [`insert_column`](api/polars.DataFrame.insert_column.html#polars.DataFrame.insert_column)Insert a Series (or expression) at a certain column index. [`interpolate`](api/polars.DataFrame.interpolate.html#polars.DataFrame.interpolate)Interpolate intermediate values. [`is_duplicated`](api/polars.DataFrame.is_duplicated.html#polars.DataFrame.is_duplicated)Get a mask of all duplicated rows in this DataFrame. [`is_empty`](api/polars.DataFrame.is_empty.html#polars.DataFrame.is_empty)Returns `True` if the DataFrame contains no rows.[`is_sorted`](api/polars.DataFrame.is_sorted.html#polars.DataFrame.is_sorted)Check whether the DataFrame is sorted by the given columns. [`is_unique`](api/polars.DataFrame.is_unique.html#polars.DataFrame.is_unique)Get a mask of all unique rows in this DataFrame. [`item`](api/polars.DataFrame.item.html#polars.DataFrame.item)Return the DataFrame as a scalar, or return the element at the given row/column. [`iter_columns`](api/polars.DataFrame.iter_columns.html#polars.DataFrame.iter_columns)Returns an iterator over the columns of this DataFrame. [`iter_rows`](api/polars.DataFrame.iter_rows.html#polars.DataFrame.iter_rows)Returns an iterator over the DataFrame of rows of python-native values. [`iter_slices`](api/polars.DataFrame.iter_slices.html#polars.DataFrame.iter_slices)Returns a non-copying iterator of slices over the underlying DataFrame. [`join`](api/polars.DataFrame.join.html#polars.DataFrame.join)Join in SQL-like fashion. [`join_asof`](api/polars.DataFrame.join_asof.html#polars.DataFrame.join_asof)Perform an asof join. [`join_where`](api/polars.DataFrame.join_where.html#polars.DataFrame.join_where)Perform a join based on one or multiple (in)equality predicates. [`lazy`](api/polars.DataFrame.lazy.html#polars.DataFrame.lazy)Start a lazy query from this point. [`limit`](api/polars.DataFrame.limit.html#polars.DataFrame.limit)Get the first `n` rows.[`map_columns`](api/polars.DataFrame.map_columns.html#polars.DataFrame.map_columns)Apply eager functions to columns of a DataFrame. [`map_rows`](api/polars.DataFrame.map_rows.html#polars.DataFrame.map_rows)Apply a custom/user-defined function (UDF) over the rows of the DataFrame. [`match_to_schema`](api/polars.DataFrame.match_to_schema.html#polars.DataFrame.match_to_schema)Match or evolve the schema of a LazyFrame into a specific schema. [`max`](api/polars.DataFrame.max.html#polars.DataFrame.max)Aggregate the columns of this DataFrame to their maximum value. [`max_horizontal`](api/polars.DataFrame.max_horizontal.html#polars.DataFrame.max_horizontal)Get the maximum value horizontally across columns. [`mean`](api/polars.DataFrame.mean.html#polars.DataFrame.mean)Aggregate the columns of this DataFrame to their mean value. [`mean_horizontal`](api/polars.DataFrame.mean_horizontal.html#polars.DataFrame.mean_horizontal)Take the mean of all values horizontally across columns. [`median`](api/polars.DataFrame.median.html#polars.DataFrame.median)Aggregate the columns of this DataFrame to their median value. [`melt`](api/polars.DataFrame.melt.html#polars.DataFrame.melt)Unpivot a DataFrame from wide to long format. [`merge_sorted`](api/polars.DataFrame.merge_sorted.html#polars.DataFrame.merge_sorted)Take two sorted DataFrames and merge them by the sorted key. [`min`](api/polars.DataFrame.min.html#polars.DataFrame.min)Aggregate the columns of this DataFrame to their minimum value. [`min_horizontal`](api/polars.DataFrame.min_horizontal.html#polars.DataFrame.min_horizontal)Get the minimum value horizontally across columns. [`n_chunks`](api/polars.DataFrame.n_chunks.html#polars.DataFrame.n_chunks)Get number of chunks used by the ChunkedArrays of this DataFrame. [`n_unique`](api/polars.DataFrame.n_unique.html#polars.DataFrame.n_unique)Return the number of unique rows, or the number of unique row-subsets. [`null_count`](api/polars.DataFrame.null_count.html#polars.DataFrame.null_count)Create a new DataFrame that shows the null counts per column. [`partition_by`](api/polars.DataFrame.partition_by.html#polars.DataFrame.partition_by)Group by the given columns and return the groups as separate dataframes. [`pipe`](api/polars.DataFrame.pipe.html#polars.DataFrame.pipe)Offers a structured way to apply a sequence of user-defined functions (UDFs). [`pivot`](api/polars.DataFrame.pivot.html#polars.DataFrame.pivot)Create a spreadsheet-style pivot table as a DataFrame. [`product`](api/polars.DataFrame.product.html#polars.DataFrame.product)Aggregate the columns of this DataFrame to their product values. [`quantile`](api/polars.DataFrame.quantile.html#polars.DataFrame.quantile)Aggregate the columns of this DataFrame to their quantile value. [`rechunk`](api/polars.DataFrame.rechunk.html#polars.DataFrame.rechunk)Rechunk the data in this DataFrame to a contiguous allocation. [`remove`](api/polars.DataFrame.remove.html#polars.DataFrame.remove)Remove rows, dropping those that match the given predicate expression(s). [`rename`](api/polars.DataFrame.rename.html#polars.DataFrame.rename)Rename column names. [`replace_column`](api/polars.DataFrame.replace_column.html#polars.DataFrame.replace_column)Replace a column at an index location. [`reverse`](api/polars.DataFrame.reverse.html#polars.DataFrame.reverse)Reverse the DataFrame. [`rolling`](api/polars.DataFrame.rolling.html#polars.DataFrame.rolling)Create rolling groups based on a temporal or integer column. [`row`](api/polars.DataFrame.row.html#polars.DataFrame.row)Get the values of a single row, either by index or by predicate. [`rows`](api/polars.DataFrame.rows.html#polars.DataFrame.rows)Returns all data in the DataFrame as a list of rows of python-native values. [`rows_by_key`](api/polars.DataFrame.rows_by_key.html#polars.DataFrame.rows_by_key)Returns all data as a dictionary of python-native values keyed by some column. [`sample`](api/polars.DataFrame.sample.html#polars.DataFrame.sample)Sample from this DataFrame. [`select`](api/polars.DataFrame.select.html#polars.DataFrame.select)Select columns from this DataFrame. [`select_seq`](api/polars.DataFrame.select_seq.html#polars.DataFrame.select_seq)Select columns from this DataFrame. [`serialize`](api/polars.DataFrame.serialize.html#polars.DataFrame.serialize)Serialize this DataFrame to a file or string in JSON format. [`set_sorted`](api/polars.DataFrame.set_sorted.html#polars.DataFrame.set_sorted)Flag a column as sorted. [`shift`](api/polars.DataFrame.shift.html#polars.DataFrame.shift)Shift values by the given number of indices. [`show`](api/polars.DataFrame.show.html#polars.DataFrame.show)Show the first `n` rows.[`shrink_to_fit`](api/polars.DataFrame.shrink_to_fit.html#polars.DataFrame.shrink_to_fit)Shrink DataFrame memory usage. [`slice`](api/polars.DataFrame.slice.html#polars.DataFrame.slice)Get a slice of this DataFrame. [`sort`](api/polars.DataFrame.sort.html#polars.DataFrame.sort)Sort the dataframe by the given columns. [`sql`](api/polars.DataFrame.sql.html#polars.DataFrame.sql)Execute a SQL query against the DataFrame. [`std`](api/polars.DataFrame.std.html#polars.DataFrame.std)Aggregate the columns of this DataFrame to their standard deviation value. [`sum`](api/polars.DataFrame.sum.html#polars.DataFrame.sum)Aggregate the columns of this DataFrame to their sum value. [`sum_horizontal`](api/polars.DataFrame.sum_horizontal.html#polars.DataFrame.sum_horizontal)Sum all values horizontally across columns. [`tail`](api/polars.DataFrame.tail.html#polars.DataFrame.tail)Get the last `n` rows.[`to_arrow`](api/polars.DataFrame.to_arrow.html#polars.DataFrame.to_arrow)Collect the underlying arrow arrays in an Arrow Table. [`to_dict`](api/polars.DataFrame.to_dict.html#polars.DataFrame.to_dict)Convert DataFrame to a dictionary mapping column name to values. [`to_dicts`](api/polars.DataFrame.to_dicts.html#polars.DataFrame.to_dicts)Convert every row to a dictionary of Python-native values. [`to_dummies`](api/polars.DataFrame.to_dummies.html#polars.DataFrame.to_dummies)Convert categorical variables into dummy/indicator variables. [`to_init_repr`](api/polars.DataFrame.to_init_repr.html#polars.DataFrame.to_init_repr)Convert DataFrame to instantiable string representation. [`to_jax`](api/polars.DataFrame.to_jax.html#polars.DataFrame.to_jax)Convert DataFrame to a Jax Array, or dict of Jax Arrays. [`to_numpy`](api/polars.DataFrame.to_numpy.html#polars.DataFrame.to_numpy)Convert this DataFrame to a NumPy ndarray. [`to_pandas`](api/polars.DataFrame.to_pandas.html#polars.DataFrame.to_pandas)Convert this DataFrame to a pandas DataFrame. [`to_series`](api/polars.DataFrame.to_series.html#polars.DataFrame.to_series)Select column as Series at index location. [`to_struct`](api/polars.DataFrame.to_struct.html#polars.DataFrame.to_struct)Convert a `DataFrame` to a`Series` of type`Struct` .[`to_torch`](api/polars.DataFrame.to_torch.html#polars.DataFrame.to_torch)Convert DataFrame to a PyTorch Tensor, Dataset, or dict of Tensors. [`top_k`](api/polars.DataFrame.top_k.html#polars.DataFrame.top_k)Return the `k` largest rows.[`transpose`](api/polars.DataFrame.transpose.html#polars.DataFrame.transpose)Transpose a DataFrame over the diagonal. [`unique`](api/polars.DataFrame.unique.html#polars.DataFrame.unique)Drop duplicate rows from this DataFrame. [`unnest`](api/polars.DataFrame.unnest.html#polars.DataFrame.unnest)Decompose struct columns into separate columns for each of their fields. [`unpivot`](api/polars.DataFrame.unpivot.html#polars.DataFrame.unpivot)Unpivot a DataFrame from wide to long format. [`unstack`](api/polars.DataFrame.unstack.html#polars.DataFrame.unstack)Unstack a long table to a wide form without doing an aggregation. [`update`](api/polars.DataFrame.update.html#polars.DataFrame.update)Update the values in this `DataFrame` with the values in`other` .[`upsample`](api/polars.DataFrame.upsample.html#polars.DataFrame.upsample)Upsample a DataFrame at a regular frequency. [`var`](api/polars.DataFrame.var.html#polars.DataFrame.var)Aggregate the columns of this DataFrame to their variance value. [`vstack`](api/polars.DataFrame.vstack.html#polars.DataFrame.vstack)Grow this DataFrame vertically by stacking a DataFrame to it. [`with_columns`](api/polars.DataFrame.with_columns.html#polars.DataFrame.with_columns)Add columns to this DataFrame. [`with_columns_seq`](api/polars.DataFrame.with_columns_seq.html#polars.DataFrame.with_columns_seq)Add columns to this DataFrame. [`with_row_count`](api/polars.DataFrame.with_row_count.html#polars.DataFrame.with_row_count)Add a column at index 0 that counts the rows. [`with_row_index`](api/polars.DataFrame.with_row_index.html#polars.DataFrame.with_row_index)Add a row index as the first column in the DataFrame. [`write_avro`](../api/polars.DataFrame.write_avro.html#polars.DataFrame.write_avro)Write to Apache Avro file. [`write_clipboard`](../api/polars.DataFrame.write_clipboard.html#polars.DataFrame.write_clipboard)Copy `DataFrame` in csv format to the system clipboard with`write_csv` .[`write_csv`](../api/polars.DataFrame.write_csv.html#polars.DataFrame.write_csv)Write to comma-separated values (CSV) file. [`write_database`](../api/polars.DataFrame.write_database.html#polars.DataFrame.write_database)Write the data in a Polars DataFrame to a database. [`write_delta`](../api/polars.DataFrame.write_delta.html#polars.DataFrame.write_delta)Write DataFrame as delta table. [`write_excel`](../api/polars.DataFrame.write_excel.html#polars.DataFrame.write_excel)Write frame data to a table in an Excel workbook/worksheet. [`write_iceberg`](../api/polars.DataFrame.write_iceberg.html#polars.DataFrame.write_iceberg)Write DataFrame to an Iceberg table. [`write_ipc`](../api/polars.DataFrame.write_ipc.html#polars.DataFrame.write_ipc)Write to Arrow IPC binary stream or Feather file. [`write_ipc_stream`](../api/polars.DataFrame.write_ipc_stream.html#polars.DataFrame.write_ipc_stream)Write to Arrow IPC record batch stream. [`write_json`](../api/polars.DataFrame.write_json.html#polars.DataFrame.write_json)Serialize to JSON representation. [`write_ndjson`](../api/polars.DataFrame.write_ndjson.html#polars.DataFrame.write_ndjson)Serialize to newline delimited JSON representation. [`write_parquet`](../api/polars.DataFrame.write_parquet.html#polars.DataFrame.write_parquet)Write to Apache Parquet file. **Attributes:**[`columns`](api/polars.DataFrame.columns.html#polars.DataFrame.columns)Get or set column names. [`dtypes`](api/polars.DataFrame.dtypes.html#polars.DataFrame.dtypes)Get the column data types. [`flags`](api/polars.DataFrame.flags.html#polars.DataFrame.flags)Get flags that are set on the columns of this DataFrame. [`height`](api/polars.DataFrame.height.html#polars.DataFrame.height)Get the number of rows. [`plot`](plot.html#polars.DataFrame.plot)Create a plot namespace. [`schema`](api/polars.DataFrame.schema.html#polars.DataFrame.schema)Get an ordered mapping of column names to their data type. [`shape`](api/polars.DataFrame.shape.html#polars.DataFrame.shape)Get the shape of the DataFrame. [`style`](style.html#polars.DataFrame.style)Create a Great Table for styling. [`width`](api/polars.DataFrame.width.html#polars.DataFrame.width)Get the number of columns. 
  - 
approx_n_unique()  DataFrame[\[source\]](https://github.com/pola-rs/polars/blob/py-1.43.2/py-polars/src/../src/polars/dataframe/frame.py#L11514-L11549)
  - Approximate count of unique values. Deprecated since version 0.20.11: Use the `select(pl.all().approx_n_unique())` method instead.This is done using the HyperLogLog++ algorithm for cardinality estimation. Examples >>> df = pl.DataFrame( ... { ... "a": [1, 2, 3, 4], ... "b": [1, 2, 1, 1], ... } ... ) >>> df.approx_n_unique() shape: (1, 2) ┌─────┬─────┐ │ a ┆ b │ │ --- ┆ --- │ │ u32 ┆ u32 │ ╞═════╪═════╡ │ 4 ┆ 2 │ └─────┴─────┘
 
  - bottom_k( ) DataFrame
  - Return the `k` smallest rows.Non-null elements are always preferred over null elements, regardless of the value of `reverse` . The output is not guaranteed to be in any
particular order, call[`sort()`](api/polars.DataFrame.sort.html#polars.DataFrame.sort) after this function if you wish the
output to be sorted.Changed in version 1.0.0: The `descending` parameter was renamed`reverse` .
    - Parameters:
      - **k**
      - Number of rows to return.
      - **by**
      - Column(s) used to determine the bottom rows. Accepts expression input. Strings are parsed as column names.
      - **reverse**
      - Consider the `k` largest elements of the`by` column(s) (instead of the`k` smallest). This can be specified per column by passing a sequence of
booleans.
 See also Examples >>> df = pl.DataFrame( ... { ... "a": ["a", "b", "a", "b", "b", "c"], ... "b": [2, 1, 1, 3, 2, 1], ... } ... ) Get the rows which contain the 4 smallest values in column b. >>> df.bottom_k(4, by="b") shape: (4, 2) ┌─────┬─────┐ │ a ┆ b │ │ --- ┆ --- │ │ str ┆ i64 │ ╞═════╪═════╡ │ b ┆ 1 │ │ a ┆ 1 │ │ c ┆ 1 │ │ a ┆ 2 │ └─────┴─────┘ Get the rows which contain the 4 smallest values when sorting on column a and b. >>> df.bottom_k(4, by=["a", "b"]) shape: (4, 2) ┌─────┬─────┐ │ a ┆ b │ │ --- ┆ --- │ │ str ┆ i64 │ ╞═════╪═════╡ │ a ┆ 1 │ │ a ┆ 2 │ │ b ┆ 1 │ │ b ┆ 2 │ └─────┴─────┘
 [\[source\]](https://github.com/pola-rs/polars/blob/py-1.43.2/py-polars/src/../src/polars/dataframe/frame.py#L6370-L6457)  - cast(
  - *dtypes: Mapping[ColumnNameOrSelector | PolarsDataType, PolarsDataType | PythonDataType] | PolarsDataType | Schema* ,
  - *** ,
  - *strict: [bool](https://docs.python.org/3/library/functions.html#bool) = True* ,
  - Cast DataFrame column(s) to the specified dtype(s). 
    - Parameters:
      - **dtypes**
      - Mapping of column names (or selector) to dtypes, or a single dtype to which all columns will be cast.
      - **strict**
      - Raise if cast is invalid on rows after predicates are pushed down. If `False` , invalid casts will produce null values.
 Examples >>> from datetime import date >>> df = pl.DataFrame( ... { ... "foo": [1, 2, 3], ... "bar": [6.0, 7.0, 8.0], ... "ham": [date(2020, 1, 2), date(2021, 3, 4), date(2022, 5, 6)], ... } ... ) Cast specific frame columns to the specified dtypes: >>> df.cast({"foo": pl.Float32, "bar": pl.UInt8}) shape: (3, 3) ┌─────┬─────┬────────────┐ │ foo ┆ bar ┆ ham │ │ --- ┆ --- ┆ --- │ │ f32 ┆ u8 ┆ date │ ╞═════╪═════╪════════════╡ │ 1.0 ┆ 6 ┆ 2020-01-02 │ │ 2.0 ┆ 7 ┆ 2021-03-04 │ │ 3.0 ┆ 8 ┆ 2022-05-06 │ └─────┴─────┴────────────┘ Cast all frame columns matching one dtype (or dtype group) to another dtype: >>> df.cast({pl.Date: pl.Datetime}) shape: (3, 3) ┌─────┬─────┬─────────────────────┐ │ foo ┆ bar ┆ ham │ │ --- ┆ --- ┆ --- │ │ i64 ┆ f64 ┆ datetime[μs] │ ╞═════╪═════╪═════════════════════╡ │ 1 ┆ 6.0 ┆ 2020-01-02 00:00:00 │ │ 2 ┆ 7.0 ┆ 2021-03-04 00:00:00 │ │ 3 ┆ 8.0 ┆ 2022-05-06 00:00:00 │ └─────┴─────┴─────────────────────┘ Use selectors to define the columns being cast: >>> import polars.selectors as cs >>> df.cast({cs.numeric(): pl.UInt32, cs.temporal(): pl.String}) shape: (3, 3) ┌─────┬─────┬────────────┐ │ foo ┆ bar ┆ ham │ │ --- ┆ --- ┆ --- │ │ u32 ┆ u32 ┆ str │ ╞═════╪═════╪════════════╡ │ 1 ┆ 6 ┆ 2020-01-02 │ │ 2 ┆ 7 ┆ 2021-03-04 │ │ 3 ┆ 8 ┆ 2022-05-06 │ └─────┴─────┴────────────┘ Cast all frame columns to the specified dtype: >>> df.cast(pl.String).to_dict(as_series=False) {'foo': ['1', '2', '3'], 'bar': ['6.0', '7.0', '8.0'], 'ham': ['2020-01-02', '2021-03-04', '2022-05-06']}
 [\[source\]](https://github.com/pola-rs/polars/blob/py-1.43.2/py-polars/src/../src/polars/dataframe/frame.py#L9064-L9155)  - 
clear(*n: [int](https://docs.python.org/3/library/functions.html#int) = 0* )  DataFrame[\[source\]](https://github.com/pola-rs/polars/blob/py-1.43.2/py-polars/src/../src/polars/dataframe/frame.py#L9157-L9213)
  - Create an empty (n=0) or `n` -row null-filled (n>0) copy of the DataFrame.Returns a `n` -row null-filled DataFrame with an identical schema.`n` can be greater than the current number of rows in the DataFrame.
    - Parameters:
      - **n**
      - Number of (null-filled) rows to return in the cleared frame.
 See also 
    - [`clone`](api/polars.DataFrame.clone.html#polars.DataFrame.clone)
    - Cheap deepcopy/clone.
 Examples >>> df = pl.DataFrame( ... { ... "a": [None, 2, 3, 4], ... "b": [0.5, None, 2.5, 13], ... "c": [True, True, False, None], ... } ... ) >>> df.clear() shape: (0, 3) ┌─────┬─────┬──────┐ │ a ┆ b ┆ c │ │ --- ┆ --- ┆ --- │ │ i64 ┆ f64 ┆ bool │ ╞═════╪═════╪══════╡ └─────┴─────┴──────┘ >>> df.clear(n=2) shape: (2, 3) ┌──────┬──────┬──────┐ │ a ┆ b ┆ c │ │ --- ┆ --- ┆ --- │ │ i64 ┆ f64 ┆ bool │ ╞══════╪══════╪══════╡ │ null ┆ null ┆ null │ │ null ┆ null ┆ null │ └──────┴──────┴──────┘
 
  - 
clone()  DataFrame[\[source\]](https://github.com/pola-rs/polars/blob/py-1.43.2/py-polars/src/../src/polars/dataframe/frame.py#L9215-L9248)
  - Create a copy of this DataFrame. This is a cheap operation that does not copy data. See also 
    - [`clear`](api/polars.DataFrame.clear.html#polars.DataFrame.clear)
    - Create an empty copy of the current DataFrame, with identical schema but no data.
 Examples >>> df = pl.DataFrame( ... { ... "a": [1, 2, 3, 4], ... "b": [0.5, 4, 10, 13], ... "c": [True, True, False, True], ... } ... ) >>> df.clone() shape: (4, 3) ┌─────┬──────┬───────┐ │ a ┆ b ┆ c │ │ --- ┆ --- ┆ --- │ │ i64 ┆ f64 ┆ bool │ ╞═════╪══════╪═══════╡ │ 1 ┆ 0.5 ┆ true │ │ 2 ┆ 4.0 ┆ true │ │ 3 ┆ 10.0 ┆ false │ │ 4 ┆ 13.0 ┆ true │ └─────┴──────┴───────┘
 
  - 
collect_schema()  Schema[\[source\]](https://github.com/pola-rs/polars/blob/py-1.43.2/py-polars/src/../src/polars/dataframe/frame.py#L1654-L1695)
  - Get an ordered mapping of column names to their data type. This is an alias for the [`schema`](api/polars.DataFrame.schema.html#polars.DataFrame.schema) property.See also Notes This method is included to facilitate writing code that is generic for both DataFrame and LazyFrame. Examples Determine the schema. >>> df = pl.DataFrame( ... { ... "foo": [1, 2, 3], ... "bar": [6.0, 7.0, 8.0], ... "ham": ["a", "b", "c"], ... } ... ) >>> df.collect_schema() Schema({'foo': Int64, 'bar': Float64, 'ham': String}) Access various properties of the schema using the `Schema` object.>>> schema = df.collect_schema() >>> schema["bar"] Float64 >>> schema.names() ['foo', 'bar', 'ham'] >>> schema.dtypes() [Int64, Float64, String] >>> schema.len() 3
 
  - 
*property* columns*: [list](https://docs.python.org/3/library/stdtypes.html#list)[[str](https://docs.python.org/3/library/stdtypes.html#str)]*[\[source\]](https://github.com/pola-rs/polars/blob/py-1.43.2/py-polars/src/../src/polars/dataframe/frame.py#L876-L913)
  - Get or set column names. 
    - Returns:
      - list of str
      - A list containing the name of each column in order.
 Examples >>> df = pl.DataFrame( ... { ... "foo": [1, 2, 3], ... "bar": [6, 7, 8], ... "ham": ["a", "b", "c"], ... } ... ) >>> df.columns ['foo', 'bar', 'ham'] Set column names: >>> df.columns = ["apple", "banana", "orange"] >>> df shape: (3, 3) ┌───────┬────────┬────────┐ │ apple ┆ banana ┆ orange │ │ --- ┆ --- ┆ --- │ │ i64 ┆ i64 ┆ str │ ╞═══════╪════════╪════════╡ │ 1 ┆ 6 ┆ a │ │ 2 ┆ 7 ┆ b │ │ 3 ┆ 8 ┆ c │ └───────┴────────┴────────┘
 
  - corr( ) DataFrame
  - Return pairwise Pearson product-moment correlation coefficients between columns. See numpy `corrcoef` for more information:[https://numpy.org/doc/stable/reference/generated/numpy.corrcoef.html](https://numpy.org/doc/stable/reference/generated/numpy.corrcoef.html)
    - Parameters:
      - **label**
      - If given, a new column that contains the labels (column names) associated with each row is added, with this name.
      - ****kwargs**
      - Keyword arguments that are passed to `numpy.corrcoef` .
 Notes This functionality requires `numpy` to be installed.Examples >>> df = pl.DataFrame({"foo": [1, 2, 3], "bar": [3, 2, 1], "ham": [7, 8, 9]}) >>> df.corr() shape: (3, 3) ┌──────┬──────┬──────┐ │ foo ┆ bar ┆ ham │ │ --- ┆ --- ┆ --- │ │ f64 ┆ f64 ┆ f64 │ ╞══════╪══════╪══════╡ │ 1.0 ┆ -1.0 ┆ 1.0 │ │ -1.0 ┆ 1.0 ┆ -1.0 │ │ 1.0 ┆ -1.0 ┆ 1.0 │ └──────┴──────┴──────┘ >>> df.corr(label="cols") shape: (3, 4) ┌──────┬──────┬──────┬──────┐ │ cols ┆ foo ┆ bar ┆ ham │ │ --- ┆ --- ┆ --- ┆ --- │ │ str ┆ f64 ┆ f64 ┆ f64 │ ╞══════╪══════╪══════╪══════╡ │ foo ┆ 1.0 ┆ -1.0 ┆ 1.0 │ │ bar ┆ -1.0 ┆ 1.0 ┆ -1.0 │ │ ham ┆ 1.0 ┆ -1.0 ┆ 1.0 │ └──────┴──────┴──────┴──────┘
 [\[source\]](https://github.com/pola-rs/polars/blob/py-1.43.2/py-polars/src/../src/polars/dataframe/frame.py#L12593-L12646)  - 
count()  DataFrame[\[source\]](https://github.com/pola-rs/polars/blob/py-1.43.2/py-polars/src/../src/polars/dataframe/frame.py#L12958-L12979)
  - Return the number of non-null elements for each column. Examples >>> df = pl.DataFrame( ... {"a": [1, 2, 3, 4], "b": [1, 2, 1, None], "c": [None, None, None, None]} ... ) >>> df.count() shape: (1, 3) ┌─────┬─────┬─────┐ │ a ┆ b ┆ c │ │ --- ┆ --- ┆ --- │ │ u32 ┆ u32 ┆ u32 │ ╞═════╪═════╪═════╡ │ 4 ┆ 3 ┆ 0 │ └─────┴─────┴─────┘
 
  - describe(
  - *percentiles: Sequence[[float](https://docs.python.org/3/library/functions.html#float)] | [float](https://docs.python.org/3/library/functions.html#float) | [None](https://docs.python.org/3/library/constants.html#None) = (0.25, 0.5, 0.75)* ,
  - *** ,
  - *interpolation: QuantileMethod = 'nearest'* ,
  - Summary statistics for a DataFrame. 
    - Parameters:
      - **percentiles**
      - One or more percentiles to include in the summary statistics. All values must be in the range `[0, 1]` .
      - **interpolation** {‘nearest’, ‘higher’, ‘lower’, ‘midpoint’, ‘linear’, ‘equiprobable’}
      - Interpolation method used when calculating percentiles.
 Warning We do not guarantee the output of `describe` to be stable. It will show
statistics that we deem informative, and may be updated in the future.
Using`describe` programmatically (versus interactive exploration) is
not recommended for this reason.See also Notes The median is included by default as the 50% percentile. Examples >>> from datetime import date, time >>> df = pl.DataFrame( ... { ... "float": [1.0, 2.8, 3.0], ... "int": [40, 50, None], ... "bool": [True, False, True], ... "str": ["zz", "xx", "yy"], ... "date": [date(2020, 1, 1), date(2021, 7, 5), date(2022, 12, 31)], ... "time": [time(10, 20, 30), time(14, 45, 50), time(23, 15, 10)], ... } ... ) Show default frame statistics: >>> df.describe() shape: (9, 7) ┌────────────┬──────────┬──────────┬──────────┬──────┬─────────────────────┬──────────┐ │ statistic ┆ float ┆ int ┆ bool ┆ str ┆ date ┆ time │ │ --- ┆ --- ┆ --- ┆ --- ┆ --- ┆ --- ┆ --- │ │ str ┆ f64 ┆ f64 ┆ f64 ┆ str ┆ str ┆ str │ ╞════════════╪══════════╪══════════╪══════════╪══════╪═════════════════════╪══════════╡ │ count ┆ 3.0 ┆ 2.0 ┆ 3.0 ┆ 3 ┆ 3 ┆ 3 │ │ null_count ┆ 0.0 ┆ 1.0 ┆ 0.0 ┆ 0 ┆ 0 ┆ 0 │ │ mean ┆ 2.266667 ┆ 45.0 ┆ 0.666667 ┆ null ┆ 2021-07-02 16:00:00 ┆ 16:07:10 │ │ std ┆ 1.101514 ┆ 7.071068 ┆ null ┆ null ┆ null ┆ null │ │ min ┆ 1.0 ┆ 40.0 ┆ 0.0 ┆ xx ┆ 2020-01-01 ┆ 10:20:30 │ │ 25% ┆ 2.8 ┆ 40.0 ┆ null ┆ null ┆ 2021-07-05 ┆ 14:45:50 │ │ 50% ┆ 2.8 ┆ 50.0 ┆ null ┆ null ┆ 2021-07-05 ┆ 14:45:50 │ │ 75% ┆ 3.0 ┆ 50.0 ┆ null ┆ null ┆ 2022-12-31 ┆ 23:15:10 │ │ max ┆ 3.0 ┆ 50.0 ┆ 1.0 ┆ zz ┆ 2022-12-31 ┆ 23:15:10 │ └────────────┴──────────┴──────────┴──────────┴──────┴─────────────────────┴──────────┘ Customize which percentiles are displayed, applying linear interpolation: >>> with pl.Config(tbl_rows=12): ... df.describe( ... percentiles=[0.1, 0.3, 0.5, 0.7, 0.9], ... interpolation="linear", ... ) shape: (11, 7) ┌────────────┬──────────┬──────────┬──────────┬──────┬─────────────────────┬──────────┐ │ statistic ┆ float ┆ int ┆ bool ┆ str ┆ date ┆ time │ │ --- ┆ --- ┆ --- ┆ --- ┆ --- ┆ --- ┆ --- │ │ str ┆ f64 ┆ f64 ┆ f64 ┆ str ┆ str ┆ str │ ╞════════════╪══════════╪══════════╪══════════╪══════╪═════════════════════╪══════════╡ │ count ┆ 3.0 ┆ 2.0 ┆ 3.0 ┆ 3 ┆ 3 ┆ 3 │ │ null_count ┆ 0.0 ┆ 1.0 ┆ 0.0 ┆ 0 ┆ 0 ┆ 0 │ │ mean ┆ 2.266667 ┆ 45.0 ┆ 0.666667 ┆ null ┆ 2021-07-02 16:00:00 ┆ 16:07:10 │ │ std ┆ 1.101514 ┆ 7.071068 ┆ null ┆ null ┆ null ┆ null │ │ min ┆ 1.0 ┆ 40.0 ┆ 0.0 ┆ xx ┆ 2020-01-01 ┆ 10:20:30 │ │ 10% ┆ 1.36 ┆ 41.0 ┆ null ┆ null ┆ 2020-04-20 ┆ 11:13:34 │ │ 30% ┆ 2.08 ┆ 43.0 ┆ null ┆ null ┆ 2020-11-26 ┆ 12:59:42 │ │ 50% ┆ 2.8 ┆ 45.0 ┆ null ┆ null ┆ 2021-07-05 ┆ 14:45:50 │ │ 70% ┆ 2.88 ┆ 47.0 ┆ null ┆ null ┆ 2022-02-07 ┆ 18:09:34 │ │ 90% ┆ 2.96 ┆ 49.0 ┆ null ┆ null ┆ 2022-09-13 ┆ 21:33:18 │ │ max ┆ 3.0 ┆ 50.0 ┆ 1.0 ┆ zz ┆ 2022-12-31 ┆ 23:15:10 │ └────────────┴──────────┴──────────┴──────────┴──────┴─────────────────────┴──────────┘
 [\[source\]](https://github.com/pola-rs/polars/blob/py-1.43.2/py-polars/src/../src/polars/dataframe/frame.py#L5915-L6014)  - 
*classmethod* deserialize(
 
)  DataFrame
  - Read a serialized DataFrame from a file. 
    - Parameters:
      - **source**
      - Path to a file or a file-like object (by file-like object, we refer to objects that have a `read()` method, such as a file handler (e.g.
via builtin`open` function) or`BytesIO` ).
      - **format**
      - The format with which the DataFrame was serialized. Options: 
        - `"binary"` : Deserialize from binary format (bytes). This is the default.
        - `"json"` : Deserialize from JSON format (string).
 See also Notes Serialization is not stable across Polars versions: a LazyFrame serialized in one Polars version may not be deserializable in another Polars version. Examples >>> import io >>> df = pl.DataFrame({"a": [1, 2, 3], "b": [4.0, 5.0, 6.0]}) >>> bytes = df.serialize() >>> pl.DataFrame.deserialize(io.BytesIO(bytes)) shape: (3, 2) ┌─────┬─────┐ │ a ┆ b │ │ --- ┆ --- │ │ i64 ┆ f64 │ ╞═════╪═════╡ │ 1 ┆ 4.0 │ │ 2 ┆ 5.0 │ │ 3 ┆ 6.0 │ └─────┴─────┘
 [\[source\]](https://github.com/pola-rs/polars/blob/py-1.43.2/py-polars/src/../src/polars/dataframe/frame.py#L498-L561)  - drop(
  - **columns: ColumnNameOrSelector | Iterable[ColumnNameOrSelector]* ,
  - *strict: [bool](https://docs.python.org/3/library/functions.html#bool) = True* ,
  - Remove columns from the dataframe. 
    - Parameters:
      - ***columns**
      - Names of the columns that should be removed from the dataframe. Accepts column selector input.
      - **strict**
      - Validate that all column names exist in the current schema, and throw an exception if any do not.
 Examples Drop a single column by passing the name of that column. >>> df = pl.DataFrame( ... { ... "foo": [1, 2, 3], ... "bar": [6.0, 7.0, 8.0], ... "ham": ["a", "b", "c"], ... } ... ) >>> df.drop("ham") shape: (3, 2) ┌─────┬─────┐ │ foo ┆ bar │ │ --- ┆ --- │ │ i64 ┆ f64 │ ╞═════╪═════╡ │ 1 ┆ 6.0 │ │ 2 ┆ 7.0 │ │ 3 ┆ 8.0 │ └─────┴─────┘ Drop multiple columns by passing a list of column names. >>> df.drop(["bar", "ham"]) shape: (3, 1) ┌─────┐ │ foo │ │ --- │ │ i64 │ ╞═════╡ │ 1 │ │ 2 │ │ 3 │ └─────┘ Drop multiple columns by passing a selector. >>> import polars.selectors as cs >>> df.drop(cs.numeric()) shape: (3, 1) ┌─────┐ │ ham │ │ --- │ │ str │ ╞═════╡ │ a │ │ b │ │ c │ └─────┘ Use positional arguments to drop multiple columns. >>> df.drop("foo", "ham") shape: (3, 1) ┌─────┐ │ bar │ │ --- │ │ f64 │ ╞═════╡ │ 6.0 │ │ 7.0 │ │ 8.0 │ └─────┘
 [\[source\]](https://github.com/pola-rs/polars/blob/py-1.43.2/py-polars/src/../src/polars/dataframe/frame.py#L8939-L9028)  - 
drop_in_place(*name: [str](https://docs.python.org/3/library/stdtypes.html#str)* )  Series[\[source\]](https://github.com/pola-rs/polars/blob/py-1.43.2/py-polars/src/../src/polars/dataframe/frame.py#L9030-L9062)
  - Drop a single column in-place and return the dropped column. 
    - Parameters:
      - **name**
      - Name of the column to drop.
    - Returns:
      - Series
      - The dropped column.
 Examples >>> df = pl.DataFrame( ... { ... "foo": [1, 2, 3], ... "bar": [6, 7, 8], ... "ham": ["a", "b", "c"], ... } ... ) >>> df.drop_in_place("ham") shape: (3,) Series: 'ham' [str] [ "a" "b" "c" ]
 
  - drop_nans(
  - *subset: ColumnNameOrSelector | Collection[ColumnNameOrSelector] | [None](https://docs.python.org/3/library/constants.html#None) = None* ,
  - Drop all rows that contain one or more NaN values. The original order of the remaining rows is preserved. 
    - Parameters:
      - **subset**
      - Column name(s) for which NaN values are considered; if set to `None` (default), use all columns (note that only floating-point columns
can contain NaNs).
 See also Notes A NaN value is not the same as a null value. To drop null values, use [`drop_nulls()`](api/polars.DataFrame.drop_nulls.html#polars.DataFrame.drop_nulls) .Examples >>> df = pl.DataFrame( ... { ... "foo": [-20.5, float("nan"), 80.0], ... "bar": [float("nan"), 110.0, 25.5], ... "ham": ["xxx", "yyy", None], ... } ... ) The default behavior of this method is to drop rows where any single value in the row is NaN: >>> df.drop_nans() shape: (1, 3) ┌──────┬──────┬──────┐ │ foo ┆ bar ┆ ham │ │ --- ┆ --- ┆ --- │ │ f64 ┆ f64 ┆ str │ ╞══════╪══════╪══════╡ │ 80.0 ┆ 25.5 ┆ null │ └──────┴──────┴──────┘ This behaviour can be constrained to consider only a subset of columns, as defined by name, or with a selector. For example, dropping rows only if there is a NaN in the “bar” column: >>> df.drop_nans(subset=["bar"]) shape: (2, 3) ┌──────┬───────┬──────┐ │ foo ┆ bar ┆ ham │ │ --- ┆ --- ┆ --- │ │ f64 ┆ f64 ┆ str │ ╞══════╪═══════╪══════╡ │ NaN ┆ 110.0 ┆ yyy │ │ 80.0 ┆ 25.5 ┆ null │ └──────┴───────┴──────┘ Dropping a row only if *all* values are NaN requires a different formulation:>>> df = pl.DataFrame( ... { ... "a": [float("nan"), float("nan"), float("nan"), float("nan")], ... "b": [10.0, 2.5, float("nan"), 5.25], ... "c": [65.75, float("nan"), float("nan"), 10.5], ... } ... ) >>> df.filter(~pl.all_horizontal(pl.all().is_nan())) shape: (3, 3) ┌─────┬──────┬───────┐ │ a ┆ b ┆ c │ │ --- ┆ --- ┆ --- │ │ f64 ┆ f64 ┆ f64 │ ╞═════╪══════╪═══════╡ │ NaN ┆ 10.0 ┆ 65.75 │ │ NaN ┆ 2.5 ┆ NaN │ │ NaN ┆ 5.25 ┆ 10.5 │ └─────┴──────┴───────┘
 [\[source\]](https://github.com/pola-rs/polars/blob/py-1.43.2/py-polars/src/../src/polars/dataframe/frame.py#L6679-L6767)  - drop_nulls(
  - *subset: ColumnNameOrSelector | Collection[ColumnNameOrSelector] | [None](https://docs.python.org/3/library/constants.html#None) = None* ,
  - Drop all rows that contain one or more null values. The original order of the remaining rows is preserved. 
    - Parameters:
      - **subset**
      - Column name(s) for which null values are considered. If set to `None` (default), use all columns.
 See also Notes A null value is not the same as a NaN value. To drop NaN values, use [`drop_nans()`](api/polars.DataFrame.drop_nans.html#polars.DataFrame.drop_nans) .Examples >>> df = pl.DataFrame( ... { ... "foo": [1, 2, 3], ... "bar": [6, None, 8], ... "ham": ["a", "b", None], ... } ... ) The default behavior of this method is to drop rows where any single value of the row is null. >>> df.drop_nulls() shape: (1, 3) ┌─────┬─────┬─────┐ │ foo ┆ bar ┆ ham │ │ --- ┆ --- ┆ --- │ │ i64 ┆ i64 ┆ str │ ╞═════╪═════╪═════╡ │ 1 ┆ 6 ┆ a │ └─────┴─────┴─────┘ This behaviour can be constrained to consider only a subset of columns, as defined by name or with a selector. For example, dropping rows if there is a null in any of the integer columns: >>> import polars.selectors as cs >>> df.drop_nulls(subset=cs.integer()) shape: (2, 3) ┌─────┬─────┬──────┐ │ foo ┆ bar ┆ ham │ │ --- ┆ --- ┆ --- │ │ i64 ┆ i64 ┆ str │ ╞═════╪═════╪══════╡ │ 1 ┆ 6 ┆ a │ │ 3 ┆ 8 ┆ null │ └─────┴─────┴──────┘ Below are some additional examples that show how to drop null values based on other conditions. >>> df = pl.DataFrame( ... { ... "a": [None, None, None, None], ... "b": [1, 2, None, 1], ... "c": [1, None, None, 1], ... } ... ) >>> df shape: (4, 3) ┌──────┬──────┬──────┐ │ a ┆ b ┆ c │ │ --- ┆ --- ┆ --- │ │ null ┆ i64 ┆ i64 │ ╞══════╪══════╪══════╡ │ null ┆ 1 ┆ 1 │ │ null ┆ 2 ┆ null │ │ null ┆ null ┆ null │ │ null ┆ 1 ┆ 1 │ └──────┴──────┴──────┘ Drop a row only if all values are null: >>> df.filter(~pl.all_horizontal(pl.all().is_null())) shape: (3, 3) ┌──────┬─────┬──────┐ │ a ┆ b ┆ c │ │ --- ┆ --- ┆ --- │ │ null ┆ i64 ┆ i64 │ ╞══════╪═════╪══════╡ │ null ┆ 1 ┆ 1 │ │ null ┆ 2 ┆ null │ │ null ┆ 1 ┆ 1 │ └──────┴─────┴──────┘ Drop a column if all values are null: >>> df[[s.name for s in df if not (s.null_count() == df.height)]] shape: (4, 2) ┌──────┬──────┐ │ b ┆ c │ │ --- ┆ --- │ │ i64 ┆ i64 │ ╞══════╪══════╡ │ 1 ┆ 1 │ │ 2 ┆ null │ │ null ┆ null │ │ 1 ┆ 1 │ └──────┴──────┘
 [\[source\]](https://github.com/pola-rs/polars/blob/py-1.43.2/py-polars/src/../src/polars/dataframe/frame.py#L6769-L6888)  - 
*property* dtypes*: [list](https://docs.python.org/3/library/stdtypes.html#list)[[DataType](../api/polars.datatypes.DataType.html#polars.datatypes.DataType)]*[\[source\]](https://github.com/pola-rs/polars/blob/py-1.43.2/py-polars/src/../src/polars/dataframe/frame.py#L928-L967)
  - Get the column data types. The data types can also be found in column headers when printing the DataFrame. 
    - Returns:
      - list of DataType
      - A list containing the data type of each column in order.
 See also Examples >>> df = pl.DataFrame( ... { ... "foo": [1, 2, 3], ... "bar": [6.0, 7.0, 8.0], ... "ham": ["a", "b", "c"], ... } ... ) >>> df.dtypes [Int64, Float64, String] >>> df shape: (3, 3) ┌─────┬─────┬─────┐ │ foo ┆ bar ┆ ham │ │ --- ┆ --- ┆ --- │ │ i64 ┆ f64 ┆ str │ ╞═════╪═════╪═════╡ │ 1 ┆ 6.0 ┆ a │ │ 2 ┆ 7.0 ┆ b │ │ 3 ┆ 8.0 ┆ c │ └─────┴─────┴─────┘
 
  - equals(
  - *other: DataFrame* ,
  - *** ,
  - *null_equal: [bool](https://docs.python.org/3/library/functions.html#bool) = True* ,
  - Check whether the DataFrame is equal to another DataFrame. 
    - Parameters:
      - **other**
      - DataFrame to compare with.
      - **null_equal**
      - Consider null values as equal.
 See also Examples >>> df1 = pl.DataFrame( ... { ... "foo": [1, 2, 3], ... "bar": [6.0, 7.0, 8.0], ... "ham": ["a", "b", "c"], ... } ... ) >>> df2 = pl.DataFrame( ... { ... "foo": [3, 2, 1], ... "bar": [8.0, 7.0, 6.0], ... "ham": ["c", "b", "a"], ... } ... ) >>> df1.equals(df1) True >>> df1.equals(df2) False
 [bool](https://docs.python.org/3/library/functions.html#bool)[\[source\]](https://github.com/pola-rs/polars/blob/py-1.43.2/py-polars/src/../src/polars/dataframe/frame.py#L6459-L6496)  - 
estimated_size(*unit: SizeUnit = 'b'* )[int](https://docs.python.org/3/library/functions.html#int) |[float](https://docs.python.org/3/library/functions.html#float)[\[source\]](https://github.com/pola-rs/polars/blob/py-1.43.2/py-polars/src/../src/polars/dataframe/frame.py#L5070-L5113)
  - Return an estimation of the total (heap) allocated size of the `DataFrame` .Estimated size is given in the specified unit (bytes by default). This estimation is the sum of the size of its buffers, validity, including nested arrays. Multiple arrays may share buffers and bitmaps. Therefore, the size of 2 arrays is not the sum of the sizes computed from this function. In particular, [ `StructArray` ]’s size is an upper bound.When an array is sliced, its allocated size remains constant because the buffer unchanged. However, this function will yield a smaller number. This is because this function returns the visible size of the buffer, not its total capacity. FFI buffers are included in this estimation. 
    - Parameters:
      - **unit** {‘b’, ‘kb’, ‘mb’, ‘gb’, ‘tb’}
      - Scale the returned size to the given unit.
 Notes For data with Object dtype, the estimated size only reports the pointer size, which is a huge underestimation. Examples >>> df = pl.DataFrame( ... { ... "x": list(reversed(range(1_000_000))), ... "y": [v / 1000 for v in range(1_000_000)], ... "z": [str(v) for v in range(1_000_000)], ... }, ... schema=[("x", pl.UInt32), ("y", pl.Float64), ("z", pl.String)], ... ) >>> df.estimated_size() 17888890 >>> df.estimated_size("mb") 17.0601749420166
 
  - 
explode(*columns: ColumnNameOrSelector | Iterable[ColumnNameOrSelector], *more_columns: ColumnNameOrSelector, empty_as_null: bool = <object object>, keep_nulls: bool = True* )  DataFrame[\[source\]](https://github.com/pola-rs/polars/blob/py-1.43.2/py-polars/src/../src/polars/dataframe/frame.py#L9519-L9593)
  - Explode the dataframe to long format by exploding the given columns. 
    - Parameters:
      - **columns**
      - Column names, expressions, or a selector defining them. The underlying columns being exploded must be of the `List` or`Array` data type.
      - ***more_columns**
      - Additional names of columns to explode, specified as positional arguments.
      - **empty_as_null**
      - Explode an empty list/array into a `null` .
      - **keep_nulls**
      - Explode a `null` list/array into a`null` .
    - Returns:
      - DataFrame
 Examples >>> df = pl.DataFrame( ... { ... "letters": ["a", "a", "b", "c"], ... "numbers": [[1], [2, 3], [4, 5], [6, 7, 8]], ... } ... ) >>> df shape: (4, 2) ┌─────────┬───────────┐ │ letters ┆ numbers │ │ --- ┆ --- │ │ str ┆ list[i64] │ ╞═════════╪═══════════╡ │ a ┆ [1] │ │ a ┆ [2, 3] │ │ b ┆ [4, 5] │ │ c ┆ [6, 7, 8] │ └─────────┴───────────┘ >>> df.explode("numbers", empty_as_null=False) shape: (8, 2) ┌─────────┬─────────┐ │ letters ┆ numbers │ │ --- ┆ --- │ │ str ┆ i64 │ ╞═════════╪═════════╡ │ a ┆ 1 │ │ a ┆ 2 │ │ a ┆ 3 │ │ b ┆ 4 │ │ b ┆ 5 │ │ c ┆ 6 │ │ c ┆ 7 │ │ c ┆ 8 │ └─────────┴─────────┘
 
  - extend(
  - *other: DataFrame* ,
  - Extend the memory backed by this `DataFrame` with the values from`other` .Different from `vstack` which adds the chunks from`other` to the chunks of
this`DataFrame` ,`extend` appends the data from`other` to the underlying
memory locations and thus may cause a reallocation.If this does not cause a reallocation, the resulting data structure will not have any extra chunks and thus will yield faster queries. Prefer `extend` over`vstack` when you want to do a query after a single
append. For instance, during online operations where you add`n` rows and rerun
a query.Prefer `vstack` over`extend` when you want to append many times before
doing a query. For instance, when you read in multiple files and want to store
them in a single`DataFrame` . In the latter case, finish the sequence of`vstack` operations with a`rechunk` .
    - Parameters:
      - **other**
      - DataFrame to vertically add.
 Warning This method modifies the dataframe in-place. The dataframe is returned for convenience only. See also Examples >>> df1 = pl.DataFrame({"foo": [1, 2, 3], "bar": [4, 5, 6]}) >>> df2 = pl.DataFrame({"foo": [10, 20, 30], "bar": [40, 50, 60]}) >>> df1.extend(df2) shape: (6, 2) ┌─────┬─────┐ │ foo ┆ bar │ │ --- ┆ --- │ │ i64 ┆ i64 │ ╞═════╪═════╡ │ 1 ┆ 4 │ │ 2 ┆ 5 │ │ 3 ┆ 6 │ │ 10 ┆ 40 │ │ 20 ┆ 50 │ │ 30 ┆ 60 │ └─────┴─────┘
 [\[source\]](https://github.com/pola-rs/polars/blob/py-1.43.2/py-polars/src/../src/polars/dataframe/frame.py#L8882-L8937)  - 
fill_nan(*value: Expr | [int](https://docs.python.org/3/library/functions.html#int) | [float](https://docs.python.org/3/library/functions.html#float) | [None](https://docs.python.org/3/library/constants.html#None)* )  DataFrame[\[source\]](https://github.com/pola-rs/polars/blob/py-1.43.2/py-polars/src/../src/polars/dataframe/frame.py#L9471-L9517)
  - Fill floating point NaN values by an Expression evaluation. 
    - Parameters:
      - **value**
      - Value used to fill NaN values.
    - Returns:
      - DataFrame
      - DataFrame with NaN values replaced by the given value.
 See also Notes A NaN value is not the same as a null value. To fill null values, use [`fill_null()`](api/polars.DataFrame.fill_null.html#polars.DataFrame.fill_null) .Examples >>> df = pl.DataFrame( ... { ... "a": [1.5, 2, float("nan"), 4], ... "b": [0.5, 4, float("nan"), 13], ... } ... ) >>> df.fill_nan(99) shape: (4, 2) ┌──────┬──────┐ │ a ┆ b │ │ --- ┆ --- │ │ f64 ┆ f64 │ ╞══════╪══════╡ │ 1.5 ┆ 0.5 │ │ 2.0 ┆ 4.0 │ │ 99.0 ┆ 99.0 │ │ 4.0 ┆ 13.0 │ └──────┴──────┘
 
  - fill_null(
  - *value: Any | Expr | [None](https://docs.python.org/3/library/constants.html#None) = None* ,
  - *strategy: FillNullStrategy | [None](https://docs.python.org/3/library/constants.html#None) = None* ,
  - *limit: [int](https://docs.python.org/3/library/functions.html#int) | [None](https://docs.python.org/3/library/constants.html#None) = None* ,
  - *** ,
  - *matches_supertype: [bool](https://docs.python.org/3/library/functions.html#bool) = True* ,
  - Fill null values using the specified value or strategy. 
    - Parameters:
      - **value**
      - Value used to fill null values.
      - **strategy** {None, ‘forward’, ‘backward’, ‘min’, ‘max’, ‘mean’, ‘zero’, ‘one’}
      - Strategy used to fill null values.
      - **limit**
      - Number of consecutive null values to fill when using the ‘forward’ or ‘backward’ strategy.
      - **matches_supertype**
      - Fill all matching supertype of the fill `value` .
    - Returns:
      - DataFrame
      - DataFrame with None values replaced by the filling strategy.
 See also Notes A null value is not the same as a NaN value. To fill NaN values, use [`fill_nan()`](api/polars.DataFrame.fill_nan.html#polars.DataFrame.fill_nan) .Examples >>> df = pl.DataFrame( ... { ... "a": [1, 2, None, 4], ... "b": [0.5, 4, None, 13], ... } ... ) >>> df.fill_null(99) shape: (4, 2) ┌─────┬──────┐ │ a ┆ b │ │ --- ┆ --- │ │ i64 ┆ f64 │ ╞═════╪══════╡ │ 1 ┆ 0.5 │ │ 2 ┆ 4.0 │ │ 99 ┆ 99.0 │ │ 4 ┆ 13.0 │ └─────┴──────┘ >>> df.fill_null(strategy="forward") shape: (4, 2) ┌─────┬──────┐ │ a ┆ b │ │ --- ┆ --- │ │ i64 ┆ f64 │ ╞═════╪══════╡ │ 1 ┆ 0.5 │ │ 2 ┆ 4.0 │ │ 2 ┆ 4.0 │ │ 4 ┆ 13.0 │ └─────┴──────┘ >>> df.fill_null(strategy="max") shape: (4, 2) ┌─────┬──────┐ │ a ┆ b │ │ --- ┆ --- │ │ i64 ┆ f64 │ ╞═════╪══════╡ │ 1 ┆ 0.5 │ │ 2 ┆ 4.0 │ │ 4 ┆ 13.0 │ │ 4 ┆ 13.0 │ └─────┴──────┘ >>> df.fill_null(strategy="zero") shape: (4, 2) ┌─────┬──────┐ │ a ┆ b │ │ --- ┆ --- │ │ i64 ┆ f64 │ ╞═════╪══════╡ │ 1 ┆ 0.5 │ │ 2 ┆ 4.0 │ │ 0 ┆ 0.0 │ │ 4 ┆ 13.0 │ └─────┴──────┘
 [\[source\]](https://github.com/pola-rs/polars/blob/py-1.43.2/py-polars/src/../src/polars/dataframe/frame.py#L9367-L9469)  - filter(
  - **predicates: IntoExprColumn | Iterable[IntoExprColumn] | [bool](https://docs.python.org/3/library/functions.html#bool) | [list](https://docs.python.org/3/library/stdtypes.html#list)[[bool](https://docs.python.org/3/library/functions.html#bool)] | np.ndarray[Any, Any]* ,
  - ***constraints: Any* ,
  - Filter rows, retaining those that match the given predicate expression(s). The original order of the remaining rows is preserved. Only rows where the predicate resolves as True are retained; when the predicate result is False (or null), the row is discarded. 
    - Parameters:
      - **predicates**
      - Expression(s) that evaluate to a boolean Series.
      - **constraints**
      - Column filters; use `name = value` to filter columns by the supplied value.
Each constraint will behave the same as`pl.col(name).eq(value)` , and
be implicitly joined with the other filter conditions using`&` .
 See also Notes If you are transitioning from Pandas, and performing filter operations based on the comparison of two or more columns, please note that in Polars any comparison involving `null` values will result in a`null` result,*not* boolean True or
False. As a result, these rows will not be retained. Ensure that null values
are handled appropriately to avoid unexpected behaviour (see examples below).Examples >>> df = pl.DataFrame( ... { ... "foo": [1, 2, 3, None, 4, None, 0], ... "bar": [6, 7, 8, None, None, 9, 0], ... "ham": ["a", "b", "c", None, "d", "e", "f"], ... } ... ) Filter rows matching a condition: >>> df.filter(pl.col("foo") > 1) shape: (3, 3) ┌─────┬──────┬─────┐ │ foo ┆ bar ┆ ham │ │ --- ┆ --- ┆ --- │ │ i64 ┆ i64 ┆ str │ ╞═════╪══════╪═════╡ │ 2 ┆ 7 ┆ b │ │ 3 ┆ 8 ┆ c │ │ 4 ┆ null ┆ d │ └─────┴──────┴─────┘ Filter on multiple conditions, combined with and/or operators: >>> df.filter( ... (pl.col("foo") < 3) & (pl.col("ham") == "a"), ... ) shape: (1, 3) ┌─────┬─────┬─────┐ │ foo ┆ bar ┆ ham │ │ --- ┆ --- ┆ --- │ │ i64 ┆ i64 ┆ str │ ╞═════╪═════╪═════╡ │ 1 ┆ 6 ┆ a │ └─────┴─────┴─────┘ >>> df.filter( ... (pl.col("foo") == 1) | (pl.col("ham") == "c"), ... ) shape: (2, 3) ┌─────┬─────┬─────┐ │ foo ┆ bar ┆ ham │ │ --- ┆ --- ┆ --- │ │ i64 ┆ i64 ┆ str │ ╞═════╪═════╪═════╡ │ 1 ┆ 6 ┆ a │ │ 3 ┆ 8 ┆ c │ └─────┴─────┴─────┘ Provide multiple filters using `*args` syntax:>>> df.filter( ... pl.col("foo") <= 2, ... ~pl.col("ham").is_in(["b", "c"]), ... ) shape: (2, 3) ┌─────┬─────┬─────┐ │ foo ┆ bar ┆ ham │ │ --- ┆ --- ┆ --- │ │ i64 ┆ i64 ┆ str │ ╞═════╪═════╪═════╡ │ 1 ┆ 6 ┆ a │ │ 0 ┆ 0 ┆ f │ └─────┴─────┴─────┘ Provide multiple filters using `**kwargs` syntax:>>> df.filter(foo=2, ham="b") shape: (1, 3) ┌─────┬─────┬─────┐ │ foo ┆ bar ┆ ham │ │ --- ┆ --- ┆ --- │ │ i64 ┆ i64 ┆ str │ ╞═════╪═════╪═════╡ │ 2 ┆ 7 ┆ b │ └─────┴─────┴─────┘ Filter by comparing two columns against each other: >>> df.filter( ... pl.col("foo") == pl.col("bar"), ... ) shape: (1, 3) ┌─────┬─────┬─────┐ │ foo ┆ bar ┆ ham │ │ --- ┆ --- ┆ --- │ │ i64 ┆ i64 ┆ str │ ╞═════╪═════╪═════╡ │ 0 ┆ 0 ┆ f │ └─────┴─────┴─────┘ >>> df.filter( ... pl.col("foo") != pl.col("bar"), ... ) shape: (3, 3) ┌─────┬─────┬─────┐ │ foo ┆ bar ┆ ham │ │ --- ┆ --- ┆ --- │ │ i64 ┆ i64 ┆ str │ ╞═════╪═════╪═════╡ │ 1 ┆ 6 ┆ a │ │ 2 ┆ 7 ┆ b │ │ 3 ┆ 8 ┆ c │ └─────┴─────┴─────┘ Notice how the row with `None` values is filtered out. In order to keep the
same behavior as pandas, use:>>> df.filter( ... pl.col("foo").ne_missing(pl.col("bar")), ... ) shape: (5, 3) ┌──────┬──────┬─────┐ │ foo ┆ bar ┆ ham │ │ --- ┆ --- ┆ --- │ │ i64 ┆ i64 ┆ str │ ╞══════╪══════╪═════╡ │ 1 ┆ 6 ┆ a │ │ 2 ┆ 7 ┆ b │ │ 3 ┆ 8 ┆ c │ │ 4 ┆ null ┆ d │ │ null ┆ 9 ┆ e │ └──────┴──────┴─────┘
 [\[source\]](https://github.com/pola-rs/polars/blob/py-1.43.2/py-polars/src/../src/polars/dataframe/frame.py#L5391-L5563)  - 
*property* flags*: [dict](https://docs.python.org/3/library/stdtypes.html#dict)[[str](https://docs.python.org/3/library/stdtypes.html#str), [dict](https://docs.python.org/3/library/stdtypes.html#dict)[[str](https://docs.python.org/3/library/stdtypes.html#str), [bool](https://docs.python.org/3/library/functions.html#bool)]]*[\[source\]](https://github.com/pola-rs/polars/blob/py-1.43.2/py-polars/src/../src/polars/dataframe/frame.py#L969-L979)
  - Get flags that are set on the columns of this DataFrame. 
    - Returns:
      - dict
      - Mapping from column names to column flags.
 
  - 
fold(*operation: Callable[[Series, Series], Series]* )  Series[\[source\]](https://github.com/pola-rs/polars/blob/py-1.43.2/py-polars/src/../src/polars/dataframe/frame.py#L11657-L11748)
  - Apply a horizontal reduction on a DataFrame. This can be used to effectively determine aggregations on a row level, and can be applied to any DataType that can be supercast (cast to a similar parent type). An example of the supercast rules when applying an arithmetic operation on two DataTypes are for instance: 
    - Int8 + String = String
    - Float32 + Int64 = Float32
    - Float32 + Float64 = Float64
 
    - Parameters:
      - **operation**
      - function that takes two `Series` and returns a`Series` .
 Examples A horizontal sum operation: >>> df = pl.DataFrame( ... { ... "a": [2, 1, 3], ... "b": [1, 2, 3], ... "c": [1.0, 2.0, 3.0], ... } ... ) >>> df.fold(lambda s1, s2: s1 + s2) shape: (3,) Series: 'a' [f64] [ 4.0 5.0 9.0 ] A horizontal minimum operation: >>> df = pl.DataFrame({"a": [2, 1, 3], "b": [1, 2, 3], "c": [1.0, 2.0, 3.0]}) >>> df.fold(lambda s1, s2: s1.zip_with(s1 < s2, s2)) shape: (3,) Series: 'a' [f64] [ 1.0 1.0 3.0 ] A horizontal string concatenation: >>> df = pl.DataFrame( ... { ... "a": ["foo", "bar", None], ... "b": [1, 2, 3], ... "c": [1.0, 2.0, 3.0], ... } ... ) >>> df.fold(lambda s1, s2: s1 + s2) shape: (3,) Series: 'a' [str] [ "foo11.0" "bar22.0" null ] A horizontal boolean or, similar to a row-wise .any(): >>> df = pl.DataFrame( ... { ... "a": [False, False, True], ... "b": [False, True, False], ... } ... ) >>> df.fold(lambda s1, s2: s1 | s2) shape: (3,) Series: 'a' [bool] [ false true true ]
 
  - gather(
  - *indices: [int](https://docs.python.org/3/library/functions.html#int) | Sequence[[int](https://docs.python.org/3/library/functions.html#int)] | IntoExpr | Series | np.ndarray[Any, Any]* ,
  - *** ,
  - *null_on_oob: [bool](https://docs.python.org/3/library/functions.html#bool) = False* ,
  - Selects rows from this DataFrame at the given indices. Warning This functionality is experimental. It may be changed at any point without it being considered a breaking change. 
    - Parameters:
      - **indices**
      - The indices of the rows to select.
      - **null_on_oob**
      - If true when an index is out-of-bounds a null row will be generated instead of raising an error.
 Examples >>> df = pl.DataFrame({"x": [2, 1, 0], "s": ["foo", "bar", "baz"]}) >>> df.gather([2, 0, 0]) shape: (3, 2) ┌─────┬─────┐ │ x ┆ s │ │ --- ┆ --- │ │ i64 ┆ str │ ╞═════╪═════╡ │ 0 ┆ baz │ │ 2 ┆ foo │ │ 2 ┆ foo │ └─────┴─────┘ >>> df.gather([0, 10, 1], null_on_oob=True) shape: (3, 2) ┌──────┬──────┐ │ x ┆ s │ │ --- ┆ --- │ │ i64 ┆ str │ ╞══════╪══════╡ │ 2 ┆ foo │ │ null ┆ null │ │ 1 ┆ bar │ └──────┴──────┘
 [\[source\]](https://github.com/pola-rs/polars/blob/py-1.43.2/py-polars/src/../src/polars/dataframe/frame.py#L8628-L8684)  - 
gather_every(*n: [int](https://docs.python.org/3/library/functions.html#int)* ,*offset: [int](https://docs.python.org/3/library/functions.html#int) = 0* )  DataFrame[\[source\]](https://github.com/pola-rs/polars/blob/py-1.43.2/py-polars/src/../src/polars/dataframe/frame.py#L12325-L12361)
  - Take every nth row in the DataFrame and return as a new DataFrame. 
    - Parameters:
      - **n**
      - Gather every *n* -th row.
      - **offset**
      - Starting index.
 Examples >>> s = pl.DataFrame({"a": [1, 2, 3, 4], "b": [5, 6, 7, 8]}) >>> s.gather_every(2) shape: (2, 2) ┌─────┬─────┐ │ a ┆ b │ │ --- ┆ --- │ │ i64 ┆ i64 │ ╞═════╪═════╡ │ 1 ┆ 5 │ │ 3 ┆ 7 │ └─────┴─────┘ >>> s.gather_every(2, offset=1) shape: (2, 2) ┌─────┬─────┐ │ a ┆ b │ │ --- ┆ --- │ │ i64 ┆ i64 │ ╞═════╪═════╡ │ 2 ┆ 6 │ │ 4 ┆ 8 │ └─────┴─────┘
 
  - get_column(
  - *name: str* ,
  - *** ,
  - *default: Any | NoDefault = <no_default>* ,
  - Get a single column by name. 
    - Parameters:
      - **name**
      - String name of the column to retrieve.
      - **default**
      - Value to return if the column does not exist; if not explicitly set and the column is not present a `ColumnNotFoundError` exception is raised.
    - Returns:
      - Series (or arbitrary default value, if specified).
 See also Examples >>> df = pl.DataFrame({"foo": [1, 2, 3], "bar": [4, 5, 6]}) >>> df.get_column("foo") shape: (3,) Series: 'foo' [i64] [ 1 2 3 ] Missing column handling; can optionally provide an arbitrary default value to the method (otherwise a `ColumnNotFoundError` exception is raised).>>> df.get_column("baz", default=pl.Series("baz", ["?", "?", "?"])) shape: (3,) Series: 'baz' [str] [ "?" "?" "?" ] >>> res = df.get_column("baz", default=None) >>> res is None True
 [\[source\]](https://github.com/pola-rs/polars/blob/py-1.43.2/py-polars/src/../src/polars/dataframe/frame.py#L9311-L9365)  - 
get_column_index(*name: [str](https://docs.python.org/3/library/stdtypes.html#str)* )[int](https://docs.python.org/3/library/functions.html#int)[\[source\]](https://github.com/pola-rs/polars/blob/py-1.43.2/py-polars/src/../src/polars/dataframe/frame.py#L6016-L6035)
  - Find the index of a column by name. 
    - Parameters:
      - **name**
      - Name of the column to find.
 Examples >>> df = pl.DataFrame( ... {"foo": [1, 2, 3], "bar": [6, 7, 8], "ham": ["a", "b", "c"]} ... ) >>> df.get_column_index("ham") 2 >>> df.get_column_index("sandwich") ColumnNotFoundError: sandwich
 
  - 
get_columns()  [list](https://docs.python.org/3/library/stdtypes.html#list) [Series][\[source\]](https://github.com/pola-rs/polars/blob/py-1.43.2/py-polars/src/../src/polars/dataframe/frame.py#L9250-L9303)
  - Get the DataFrame as a List of Series. Examples >>> df = pl.DataFrame({"foo": [1, 2, 3], "bar": [4, 5, 6]}) >>> df.get_columns() [shape: (3,) Series: 'foo' [i64] [ 1 2 3 ], shape: (3,) Series: 'bar' [i64] [ 4 5 6 ]] >>> df = pl.DataFrame( ... { ... "a": [1, 2, 3, 4], ... "b": [0.5, 4, 10, 13], ... "c": [True, True, False, True], ... } ... ) >>> df.get_columns() [shape: (4,) Series: 'a' [i64] [ 1 2 3 4 ], shape: (4,) Series: 'b' [f64] [ 0.5 4.0 10.0 13.0 ], shape: (4,) Series: 'c' [bool] [ true true false true ]]
 
  - glimpse(
  - *** ,
  - *max_items_per_column: [int](https://docs.python.org/3/library/functions.html#int) = 10* ,
  - *max_colname_length: [int](https://docs.python.org/3/library/functions.html#int) = 50* ,
  - *return_type: Literal['frame', 'self', 'string'] | [None](https://docs.python.org/3/library/constants.html#None) = None* ,
  - Return a dense preview of the DataFrame. The formatting shows one line per column so that wide dataframes display cleanly. Each line shows the column name, the data type, and the first few values. Changed in version 1.35.0: The `return_as_string` parameter was renamed`return_type` and now accepts
string values`'string'` and`'frame'` instead of boolean True or False.
    - Parameters:
      - **max_items_per_column**
      - Maximum number of items to show per column.
      - **max_colname_length**
      - Maximum length of the displayed column names; values that exceed this value are truncated with a trailing ellipsis.
      - **return_type**
      - Modify the return format: 
        - `None` (default): Print the glimpse output to stdout, returning`None` .
        - `"self"` : Print the glimpse output to stdout, returning the*original* frame.
        - `"frame"` : Return the glimpse output as a new DataFrame.
        - `"string"` : Return the glimpse output as a string.
 Examples >>> from datetime import date >>> df = pl.DataFrame( ... { ... "a": [1.0, 2.8, 3.0], ... "b": [4, 5, None], ... "c": [True, False, True], ... "d": [None, "b", "c"], ... "e": ["usd", "eur", None], ... "f": [date(2020, 1, 1), date(2021, 1, 2), date(2022, 1, 1)], ... } ... ) Print glimpse-formatted output to stdout, returning `None` :>>> res = df.glimpse() Rows: 3 Columns: 6 $ a <f64> 1.0, 2.8, 3.0 $ b <i64> 4, 5, null $ c <bool> True, False, True $ d <str> null, 'b', 'c' $ e <str> 'usd', 'eur', null $ f <date> 2020-01-01, 2021-01-02, 2022-01-01 >>> res is None True Return the glimpse output as a string: >>> res = df.glimpse(return_type="string") >>> isinstance(res, str) True Return the glimpse output as a DataFrame: >>> df.glimpse(return_type="frame") shape: (6, 3) ┌────────┬───────┬─────────────────────────────────┐ │ column ┆ dtype ┆ values │ │ --- ┆ --- ┆ --- │ │ str ┆ str ┆ list[str] │ ╞════════╪═══════╪═════════════════════════════════╡ │ a ┆ f64 ┆ ["1.0", "2.8", "3.0"] │ │ b ┆ i64 ┆ ["4", "5", null] │ │ c ┆ bool ┆ ["True", "False", "True"] │ │ d ┆ str ┆ [null, "'b'", "'c'"] │ │ e ┆ str ┆ ["'usd'", "'eur'", null] │ │ f ┆ date ┆ ["2020-01-01", "2021-01-02", "… │ └────────┴───────┴─────────────────────────────────┘ Print glimpse-formatted output to stdout, returning the *original* frame:>>> res = df.glimpse(return_type="self") Rows: 3 Columns: 6 $ a <f64> 1.0, 2.8, 3.0 $ b <i64> 4, 5, null $ c <bool> True, False, True $ d <str> null, 'b', 'c' $ e <str> 'usd', 'eur', null $ f <date> 2020-01-01, 2021-01-02, 2022-01-01 >>> res shape: (3, 6) ┌─────┬──────┬───────┬──────┬──────┬────────────┐ │ a ┆ b ┆ c ┆ d ┆ e ┆ f │ │ --- ┆ --- ┆ --- ┆ --- ┆ --- ┆ --- │ │ f64 ┆ i64 ┆ bool ┆ str ┆ str ┆ date │ ╞═════╪══════╪═══════╪══════╪══════╪════════════╡ │ 1.0 ┆ 4 ┆ true ┆ null ┆ usd ┆ 2020-01-01 │ │ 2.8 ┆ 5 ┆ false ┆ b ┆ eur ┆ 2021-01-02 │ │ 3.0 ┆ null ┆ true ┆ c ┆ null ┆ 2022-01-01 │ └─────┴──────┴───────┴──────┴──────┴────────────┘
 [str](https://docs.python.org/3/library/stdtypes.html#str) | DataFrame |[None](https://docs.python.org/3/library/constants.html#None)[\[source\]](https://github.com/pola-rs/polars/blob/py-1.43.2/py-polars/src/../src/polars/dataframe/frame.py#L5739-L5913)  - group_by(
  - **by: IntoExpr | Iterable[IntoExpr]* ,
  - *maintain_order: [bool](https://docs.python.org/3/library/functions.html#bool) = False* ,
  - ***named_by: IntoExpr* ,
  - Start a group by operation. 
    - Parameters:
      - ***by**
      - Column(s) to group by. Accepts expression input. Strings are parsed as column names.
      - **maintain_order**
      - Ensure that the order of the groups is consistent with the input data. This is slower than a default group by. Settings this to `True` blocks the possibility
to run on the streaming engine.Note Within each group, the order of rows is always preserved, regardless of this argument.
      - ****named_by**
      - Additional columns to group by, specified as keyword arguments. The columns will be renamed to the keyword used.
    - Returns:
      - GroupBy
      - Object which can be used to perform aggregations.
 Examples Group by one column and call `agg` to compute the grouped sum of another
column.>>> df = pl.DataFrame( ... { ... "a": ["a", "b", "a", "b", "c"], ... "b": [1, 2, 1, 3, 3], ... "c": [5, 4, 3, 2, 1], ... } ... ) >>> df.group_by("a").agg(pl.col("b").sum()) shape: (3, 2) ┌─────┬─────┐ │ a ┆ b │ │ --- ┆ --- │ │ str ┆ i64 │ ╞═════╪═════╡ │ a ┆ 2 │ │ b ┆ 5 │ │ c ┆ 3 │ └─────┴─────┘ Set `maintain_order=True` to ensure the order of the groups is consistent with
the input.>>> df.group_by("a", maintain_order=True).agg(pl.col("c")) shape: (3, 2) ┌─────┬───────────┐ │ a ┆ c │ │ --- ┆ --- │ │ str ┆ list[i64] │ ╞═════╪═══════════╡ │ a ┆ [5, 3] │ │ b ┆ [4, 2] │ │ c ┆ [1] │ └─────┴───────────┘ Group by multiple columns by passing a list of column names. >>> df.group_by(["a", "b"]).agg(pl.max("c")) shape: (4, 3) ┌─────┬─────┬─────┐ │ a ┆ b ┆ c │ │ --- ┆ --- ┆ --- │ │ str ┆ i64 ┆ i64 │ ╞═════╪═════╪═════╡ │ a ┆ 1 ┆ 5 │ │ b ┆ 2 ┆ 4 │ │ b ┆ 3 ┆ 2 │ │ c ┆ 3 ┆ 1 │ └─────┴─────┴─────┘ Or use positional arguments to group by multiple columns in the same way. Expressions are also accepted. >>> df.group_by("a", pl.col("b") // 2).agg(pl.col("c").mean()) shape: (3, 3) ┌─────┬─────┬─────┐ │ a ┆ b ┆ c │ │ --- ┆ --- ┆ --- │ │ str ┆ i64 ┆ f64 │ ╞═════╪═════╪═════╡ │ a ┆ 0 ┆ 4.0 │ │ b ┆ 1 ┆ 3.0 │ │ c ┆ 1 ┆ 1.0 │ └─────┴─────┴─────┘ The `GroupBy` object returned by this method is iterable, returning the name
and data of each group.>>> for name, data in df.group_by("a"): ... print(name) ... print(data) ('a',) shape: (2, 3) ┌─────┬─────┬─────┐ │ a ┆ b ┆ c │ │ --- ┆ --- ┆ --- │ │ str ┆ i64 ┆ i64 │ ╞═════╪═════╪═════╡ │ a ┆ 1 ┆ 5 │ │ a ┆ 1 ┆ 3 │ └─────┴─────┴─────┘ ('b',) shape: (2, 3) ┌─────┬─────┬─────┐ │ a ┆ b ┆ c │ │ --- ┆ --- ┆ --- │ │ str ┆ i64 ┆ i64 │ ╞═════╪═════╪═════╡ │ b ┆ 2 ┆ 4 │ │ b ┆ 3 ┆ 2 │ └─────┴─────┴─────┘ ('c',) shape: (1, 3) ┌─────┬─────┬─────┐ │ a ┆ b ┆ c │ │ --- ┆ --- ┆ --- │ │ str ┆ i64 ┆ i64 │ ╞═════╪═════╪═════╡ │ c ┆ 3 ┆ 1 │ └─────┴─────┴─────┘
 [\[source\]](https://github.com/pola-rs/polars/blob/py-1.43.2/py-polars/src/../src/polars/dataframe/frame.py#L7163-L7312)  - group_by_dynamic(
  - *index_column: IntoExpr* ,
  - *** ,
  - *every: [str](https://docs.python.org/3/library/stdtypes.html#str) | timedelta* ,
  - *period: [str](https://docs.python.org/3/library/stdtypes.html#str) | timedelta | [None](https://docs.python.org/3/library/constants.html#None) = None* ,
  - *offset: [str](https://docs.python.org/3/library/stdtypes.html#str) | timedelta | [None](https://docs.python.org/3/library/constants.html#None) = None* ,
  - *include_boundaries: [bool](https://docs.python.org/3/library/functions.html#bool) = False* ,
  - *closed: ClosedInterval = 'left'* ,
  - *label: Label = 'left'* ,
  - *group_by: IntoExpr | Iterable[IntoExpr] | [None](https://docs.python.org/3/library/constants.html#None) = None* ,
  - *start_by: StartBy = 'window'* ,
  - Group based on a time value (or index value of type Int32, Int64). Time windows are calculated and rows are assigned to windows. Different from a normal group by is that a row can be member of multiple groups. By default, the windows look like: 
    - [start, start + period)
    - [start + every, start + every + period)
    - [start + 2*every, start + 2*every + period)
    - …
 where `start` is determined by`start_by` ,`offset` ,`every` , and the earliest
datapoint. See the`start_by` argument description for details.Warning The index column must be sorted in ascending order. If `group_by` is passed, then
the index column must be sorted in ascending order within each group.Changed in version 0.20.14: The `by` parameter was renamed`group_by` .
    - Parameters:
      - **index_column**
      - Column used to group based on the time window. Often of type Date/Datetime. This column must be sorted in ascending order (or, if `group_by` is specified,
then it must be sorted in ascending order within each group).In case of a dynamic group by on indices, dtype needs to be one of {Int32, Int64}. Note that Int32 gets temporarily cast to Int64, so if performance matters use an Int64 column.
      - **every**
      - interval of the window
      - **period**
      - length of the window, if None it will equal ‘every’
      - **offset**
      - offset of the window, does not take effect if `start_by` is ‘datapoint’.
Defaults to zero.
      - **include_boundaries**
      - Add the lower and upper bound of the window to the “_lower_boundary” and “_upper_boundary” columns. This will impact performance because it’s harder to parallelize
      - **closed** {‘left’, ‘right’, ‘both’, ‘none’}
      - Define which sides of the temporal interval are closed (inclusive).
      - **label** {‘left’, ‘right’, ‘datapoint’}
      - Define which label to use for the window: 
        - ‘left’: lower boundary of the window
        - ‘right’: upper boundary of the window
        - ‘datapoint’: the first value of the index column in the given window. If you don’t need the label to be at one of the boundaries, choose this option for maximum performance
      - **group_by**
      - Also group by this column/these columns
      - **start_by** {‘window’, ‘datapoint’, ‘monday’, ‘tuesday’, ‘wednesday’, ‘thursday’, ‘friday’, ‘saturday’, ‘sunday’}
      - The strategy to determine the start of the first window by. 
        - ‘window’: Start by taking the earliest timestamp, truncating it with `every` , and then adding`offset` .
Note that weekly windows start on Monday.
        - ‘datapoint’: Start from the first encountered data point.
        - a day of the week (only takes effect if `every` contains`'w'` ):
          - ‘monday’: Start the window on the Monday before the first data point.
          - ‘tuesday’: Start the window on the Tuesday before the first data point.
          - …
          - ‘sunday’: Start the window on the Sunday before the first data point.
 The resulting window is then shifted back until the earliest datapoint is in or in front of it.
    - Returns:
      - DynamicGroupBy
      - Object you can call `.agg` on to aggregate by groups, the result
of which will be sorted by`index_column` (but note that if`group_by` columns are
passed, it will only be sorted within each group).
 See also Notes 
    1. If you’re coming from pandas, then # polars df.group_by_dynamic("ts", every="1d").agg(pl.col("value").sum()) is equivalent to # pandas df.set_index("ts").resample("D")["value"].sum().reset_index() though note that, unlike pandas, polars doesn’t add extra rows for empty windows. If you need `index_column` to be evenly spaced, then please combine
with[`DataFrame.upsample()`](api/polars.DataFrame.upsample.html#polars.DataFrame.upsample) .
    2. The `every` ,`period` and`offset` arguments are created with
the following string language:
      - 1ns (1 nanosecond)
      - 1us (1 microsecond)
      - 1ms (1 millisecond)
      - 1s (1 second)
      - 1m (1 minute)
      - 1h (1 hour)
      - 1d (1 calendar day)
      - 1w (1 calendar week)
      - 1mo (1 calendar month)
      - 1q (1 calendar quarter)
      - 1y (1 calendar year)
      - 1i (1 index count)
 Or combine them (except in `every` ):
“3d12h4m25s” # 3 days, 12 hours, 4 minutes, and 25 secondsBy “calendar day”, we mean the corresponding time on the next day (which may not be 24 hours, due to daylight savings). Similarly for “calendar week”, “calendar month”, “calendar quarter”, and “calendar year”. In case of a group_by_dynamic on an integer column, the windows are defined by: 
      - “1i” # length 1
      - “10i” # length 10
 Examples >>> from datetime import datetime >>> df = pl.DataFrame( ... { ... "time": pl.datetime_range( ... start=datetime(2021, 12, 16), ... end=datetime(2021, 12, 16, 3), ... interval="30m", ... eager=True, ... ), ... "n": range(7), ... } ... ) >>> df shape: (7, 2) ┌─────────────────────┬─────┐ │ time ┆ n │ │ --- ┆ --- │ │ datetime[μs] ┆ i64 │ ╞═════════════════════╪═════╡ │ 2021-12-16 00:00:00 ┆ 0 │ │ 2021-12-16 00:30:00 ┆ 1 │ │ 2021-12-16 01:00:00 ┆ 2 │ │ 2021-12-16 01:30:00 ┆ 3 │ │ 2021-12-16 02:00:00 ┆ 4 │ │ 2021-12-16 02:30:00 ┆ 5 │ │ 2021-12-16 03:00:00 ┆ 6 │ └─────────────────────┴─────┘ Group by windows of 1 hour. >>> df.group_by_dynamic("time", every="1h", closed="right").agg(pl.col("n")) shape: (4, 2) ┌─────────────────────┬───────────┐ │ time ┆ n │ │ --- ┆ --- │ │ datetime[μs] ┆ list[i64] │ ╞═════════════════════╪═══════════╡ │ 2021-12-15 23:00:00 ┆ [0] │ │ 2021-12-16 00:00:00 ┆ [1, 2] │ │ 2021-12-16 01:00:00 ┆ [3, 4] │ │ 2021-12-16 02:00:00 ┆ [5, 6] │ └─────────────────────┴───────────┘ The window boundaries can also be added to the aggregation result >>> df.group_by_dynamic( ... "time", every="1h", include_boundaries=True, closed="right" ... ).agg(pl.col("n").mean()) shape: (4, 4) ┌─────────────────────┬─────────────────────┬─────────────────────┬─────┐ │ _lower_boundary ┆ _upper_boundary ┆ time ┆ n │ │ --- ┆ --- ┆ --- ┆ --- │ │ datetime[μs] ┆ datetime[μs] ┆ datetime[μs] ┆ f64 │ ╞═════════════════════╪═════════════════════╪═════════════════════╪═════╡ │ 2021-12-15 23:00:00 ┆ 2021-12-16 00:00:00 ┆ 2021-12-15 23:00:00 ┆ 0.0 │ │ 2021-12-16 00:00:00 ┆ 2021-12-16 01:00:00 ┆ 2021-12-16 00:00:00 ┆ 1.5 │ │ 2021-12-16 01:00:00 ┆ 2021-12-16 02:00:00 ┆ 2021-12-16 01:00:00 ┆ 3.5 │ │ 2021-12-16 02:00:00 ┆ 2021-12-16 03:00:00 ┆ 2021-12-16 02:00:00 ┆ 5.5 │ └─────────────────────┴─────────────────────┴─────────────────────┴─────┘ When closed=”left”, the window excludes the right end of interval: [lower_bound, upper_bound) >>> df.group_by_dynamic("time", every="1h", closed="left").agg(pl.col("n")) shape: (4, 2) ┌─────────────────────┬───────────┐ │ time ┆ n │ │ --- ┆ --- │ │ datetime[μs] ┆ list[i64] │ ╞═════════════════════╪═══════════╡ │ 2021-12-16 00:00:00 ┆ [0, 1] │ │ 2021-12-16 01:00:00 ┆ [2, 3] │ │ 2021-12-16 02:00:00 ┆ [4, 5] │ │ 2021-12-16 03:00:00 ┆ [6] │ └─────────────────────┴───────────┘ When closed=”both” the time values at the window boundaries belong to 2 groups. >>> df.group_by_dynamic("time", every="1h", closed="both").agg(pl.col("n")) shape: (4, 2) ┌─────────────────────┬───────────┐ │ time ┆ n │ │ --- ┆ --- │ │ datetime[μs] ┆ list[i64] │ ╞═════════════════════╪═══════════╡ │ 2021-12-16 00:00:00 ┆ [0, 1, 2] │ │ 2021-12-16 01:00:00 ┆ [2, 3, 4] │ │ 2021-12-16 02:00:00 ┆ [4, 5, 6] │ │ 2021-12-16 03:00:00 ┆ [6] │ └─────────────────────┴───────────┘ Dynamic group bys can also be combined with grouping on normal keys >>> df = df.with_columns(groups=pl.Series(["a", "a", "a", "b", "b", "a", "a"])) >>> df shape: (7, 3) ┌─────────────────────┬─────┬────────┐ │ time ┆ n ┆ groups │ │ --- ┆ --- ┆ --- │ │ datetime[μs] ┆ i64 ┆ str │ ╞═════════════════════╪═════╪════════╡ │ 2021-12-16 00:00:00 ┆ 0 ┆ a │ │ 2021-12-16 00:30:00 ┆ 1 ┆ a │ │ 2021-12-16 01:00:00 ┆ 2 ┆ a │ │ 2021-12-16 01:30:00 ┆ 3 ┆ b │ │ 2021-12-16 02:00:00 ┆ 4 ┆ b │ │ 2021-12-16 02:30:00 ┆ 5 ┆ a │ │ 2021-12-16 03:00:00 ┆ 6 ┆ a │ └─────────────────────┴─────┴────────┘ >>> df.group_by_dynamic( ... "time", ... every="1h", ... closed="both", ... group_by="groups", ... include_boundaries=True, ... ).agg(pl.col("n")) shape: (6, 5) ┌────────┬─────────────────────┬─────────────────────┬─────────────────────┬───────────┐ │ groups ┆ _lower_boundary ┆ _upper_boundary ┆ time ┆ n │ │ --- ┆ --- ┆ --- ┆ --- ┆ --- │ │ str ┆ datetime[μs] ┆ datetime[μs] ┆ datetime[μs] ┆ list[i64] │ ╞════════╪═════════════════════╪═════════════════════╪═════════════════════╪═══════════╡ │ a ┆ 2021-12-16 00:00:00 ┆ 2021-12-16 01:00:00 ┆ 2021-12-16 00:00:00 ┆ [0, 1, 2] │ │ a ┆ 2021-12-16 01:00:00 ┆ 2021-12-16 02:00:00 ┆ 2021-12-16 01:00:00 ┆ [2] │ │ a ┆ 2021-12-16 02:00:00 ┆ 2021-12-16 03:00:00 ┆ 2021-12-16 02:00:00 ┆ [5, 6] │ │ a ┆ 2021-12-16 03:00:00 ┆ 2021-12-16 04:00:00 ┆ 2021-12-16 03:00:00 ┆ [6] │ │ b ┆ 2021-12-16 01:00:00 ┆ 2021-12-16 02:00:00 ┆ 2021-12-16 01:00:00 ┆ [3, 4] │ │ b ┆ 2021-12-16 02:00:00 ┆ 2021-12-16 03:00:00 ┆ 2021-12-16 02:00:00 ┆ [4] │ └────────┴─────────────────────┴─────────────────────┴─────────────────────┴───────────┘ Dynamic group by on an index column >>> df = pl.DataFrame( ... { ... "idx": pl.int_range(0, 6, eager=True), ... "A": ["A", "A", "B", "B", "B", "C"], ... } ... ) >>> ( ... df.group_by_dynamic( ... "idx", ... every="2i", ... period="3i", ... include_boundaries=True, ... closed="right", ... ).agg(pl.col("A").alias("A_agg_list")) ... ) shape: (4, 4) ┌─────────────────┬─────────────────┬─────┬─────────────────┐ │ _lower_boundary ┆ _upper_boundary ┆ idx ┆ A_agg_list │ │ --- ┆ --- ┆ --- ┆ --- │ │ i64 ┆ i64 ┆ i64 ┆ list[str] │ ╞═════════════════╪═════════════════╪═════╪═════════════════╡ │ -2 ┆ 1 ┆ -2 ┆ ["A", "A"] │ │ 0 ┆ 3 ┆ 0 ┆ ["A", "B", "B"] │ │ 2 ┆ 5 ┆ 2 ┆ ["B", "B", "C"] │ │ 4 ┆ 7 ┆ 4 ┆ ["C"] │ └─────────────────┴─────────────────┴─────┴─────────────────┘
 [\[source\]](https://github.com/pola-rs/polars/blob/py-1.43.2/py-polars/src/../src/polars/dataframe/frame.py#L7472-L7791)  - hash_rows( ) Series
  - Hash and combine the rows in this DataFrame. The hash value is of type `UInt64` .
    - Parameters:
      - **seed**
      - Random seed parameter. Defaults to 0.
      - **seed_1**
      - Random seed parameter. Defaults to `seed` if not set.
      - **seed_2**
      - Random seed parameter. Defaults to `seed` if not set.
      - **seed_3**
      - Random seed parameter. Defaults to `seed` if not set.
 Notes This implementation of `hash_rows` does not guarantee stable results
across different Polars versions. Its stability is only guaranteed within a
single version.Examples >>> df = pl.DataFrame( ... { ... "foo": [1, None, 3, 4], ... "ham": ["a", "b", None, "d"], ... } ... ) >>> df.hash_rows(seed=42) shape: (4,) Series: '' [u64] [ 10783150408545073287 1438741209321515184 10047419486152048166 2047317070637311557 ]
 [\[source\]](https://github.com/pola-rs/polars/blob/py-1.43.2/py-polars/src/../src/polars/dataframe/frame.py#L12363-L12414)  - 
head(*n: [int](https://docs.python.org/3/library/functions.html#int) = 5* )  DataFrame[\[source\]](https://github.com/pola-rs/polars/blob/py-1.43.2/py-polars/src/../src/polars/dataframe/frame.py#L6534-L6584)
  - Get the first `n` rows.
    - Parameters:
      - **n**
      - Number of rows to return. If a negative value is passed, return all rows except the last `abs(n)` .
 Examples >>> df = pl.DataFrame( ... { ... "foo": [1, 2, 3, 4, 5], ... "bar": [6, 7, 8, 9, 10], ... "ham": ["a", "b", "c", "d", "e"], ... } ... ) >>> df.head(3) shape: (3, 3) ┌─────┬─────┬─────┐ │ foo ┆ bar ┆ ham │ │ --- ┆ --- ┆ --- │ │ i64 ┆ i64 ┆ str │ ╞═════╪═════╪═════╡ │ 1 ┆ 6 ┆ a │ │ 2 ┆ 7 ┆ b │ │ 3 ┆ 8 ┆ c │ └─────┴─────┴─────┘ Pass a negative value to get all rows `except` the last`abs(n)` .>>> df.head(-3) shape: (2, 3) ┌─────┬─────┬─────┐ │ foo ┆ bar ┆ ham │ │ --- ┆ --- ┆ --- │ │ i64 ┆ i64 ┆ str │ ╞═════╪═════╪═════╡ │ 1 ┆ 6 ┆ a │ │ 2 ┆ 7 ┆ b │ └─────┴─────┴─────┘
 
  - 
*property* height*: [int](https://docs.python.org/3/library/functions.html#int)*[\[source\]](https://github.com/pola-rs/polars/blob/py-1.43.2/py-polars/src/../src/polars/dataframe/frame.py#L837-L852)
  - Get the number of rows. 
    - Returns:
      - int
 Examples >>> df = pl.DataFrame({"foo": [1, 2, 3, 4, 5]}) >>> df.height 5
 
  - hstack( ) DataFrame
  - Return a new DataFrame grown horizontally by stacking multiple Series to it. 
    - Parameters:
      - **columns**
      - Series to stack.
      - **in_place**
      - Modify in place.
 Examples >>> df = pl.DataFrame( ... { ... "foo": [1, 2, 3], ... "bar": [6, 7, 8], ... "ham": ["a", "b", "c"], ... } ... ) >>> x = pl.Series("apple", [10, 20, 30]) >>> df.hstack([x]) shape: (3, 4) ┌─────┬─────┬─────┬───────┐ │ foo ┆ bar ┆ ham ┆ apple │ │ --- ┆ --- ┆ --- ┆ --- │ │ i64 ┆ i64 ┆ str ┆ i64 │ ╞═════╪═════╪═════╪═══════╡ │ 1 ┆ 6 ┆ a ┆ 10 │ │ 2 ┆ 7 ┆ b ┆ 20 │ │ 3 ┆ 8 ┆ c ┆ 30 │ └─────┴─────┴─────┴───────┘
 [\[source\]](https://github.com/pola-rs/polars/blob/py-1.43.2/py-polars/src/../src/polars/dataframe/frame.py#L8788-L8829)  - 
insert_column(*index: [int](https://docs.python.org/3/library/functions.html#int)* ,*column: IntoExprColumn* )  DataFrame[\[source\]](https://github.com/pola-rs/polars/blob/py-1.43.2/py-polars/src/../src/polars/dataframe/frame.py#L5319-L5389)
  - Insert a Series (or expression) at a certain column index. This operation is in place. 
    - Parameters:
      - **index**
      - Index at which to insert the new column.
      - **column**
      - `Series` or expression to insert.
 Examples Insert a new Series column at the given index: >>> df = pl.DataFrame({"foo": [1, 2, 3], "bar": [4, 5, 6]}) >>> s = pl.Series("baz", [97, 98, 99]) >>> df.insert_column(1, s) shape: (3, 3) ┌─────┬─────┬─────┐ │ foo ┆ baz ┆ bar │ │ --- ┆ --- ┆ --- │ │ i64 ┆ i64 ┆ i64 │ ╞═════╪═════╪═════╡ │ 1 ┆ 97 ┆ 4 │ │ 2 ┆ 98 ┆ 5 │ │ 3 ┆ 99 ┆ 6 │ └─────┴─────┴─────┘ Insert a new expression column at the given index: >>> df = pl.DataFrame( ... {"a": [2, 4, 2], "b": [0.5, 4, 10], "c": ["xx", "yy", "zz"]} ... ) >>> expr = (pl.col("b") / pl.col("a")).alias("b_div_a") >>> df.insert_column(2, expr) shape: (3, 4) ┌─────┬──────┬─────────┬─────┐ │ a ┆ b ┆ b_div_a ┆ c │ │ --- ┆ --- ┆ --- ┆ --- │ │ i64 ┆ f64 ┆ f64 ┆ str │ ╞═════╪══════╪═════════╪═════╡ │ 2 ┆ 0.5 ┆ 0.25 ┆ xx │ │ 4 ┆ 4.0 ┆ 1.0 ┆ yy │ │ 2 ┆ 10.0 ┆ 5.0 ┆ zz │ └─────┴──────┴─────────┴─────┘
 
  - 
interpolate()  DataFrame[\[source\]](https://github.com/pola-rs/polars/blob/py-1.43.2/py-polars/src/../src/polars/dataframe/frame.py#L12416-L12444)
  - Interpolate intermediate values. The interpolation method is linear. Nulls at the beginning and end of the series remain null. Examples >>> df = pl.DataFrame( ... { ... "foo": [1, None, 9, 10], ... "bar": [6, 7, 9, None], ... "baz": [1, None, None, 9], ... } ... ) >>> df.interpolate() shape: (4, 3) ┌──────┬──────┬──────────┐ │ foo ┆ bar ┆ baz │ │ --- ┆ --- ┆ --- │ │ f64 ┆ f64 ┆ f64 │ ╞══════╪══════╪══════════╡ │ 1.0 ┆ 6.0 ┆ 1.0 │ │ 5.0 ┆ 7.0 ┆ 3.666667 │ │ 9.0 ┆ 9.0 ┆ 6.333333 │ │ 10.0 ┆ null ┆ 9.0 │ └──────┴──────┴──────────┘
 
  - 
is_duplicated()  Series[\[source\]](https://github.com/pola-rs/polars/blob/py-1.43.2/py-polars/src/../src/polars/dataframe/frame.py#L10338-L10373)
  - Get a mask of all duplicated rows in this DataFrame. Examples >>> df = pl.DataFrame( ... { ... "a": [1, 2, 3, 1], ... "b": ["x", "y", "z", "x"], ... } ... ) >>> df.is_duplicated() shape: (4,) Series: '' [bool] [ true false false true ] This mask can be used to visualize the duplicated lines like this: >>> df.filter(df.is_duplicated()) shape: (2, 2) ┌─────┬─────┐ │ a ┆ b │ │ --- ┆ --- │ │ i64 ┆ str │ ╞═════╪═════╡ │ 1 ┆ x │ │ 1 ┆ x │ └─────┴─────┘
 
  - 
is_empty()  [bool](https://docs.python.org/3/library/functions.html#bool)[\[source\]](https://github.com/pola-rs/polars/blob/py-1.43.2/py-polars/src/../src/polars/dataframe/frame.py#L12446-L12458)
  - Returns `True` if the DataFrame contains no rows.Examples >>> df = pl.DataFrame({"foo": [1, 2, 3], "bar": [4, 5, 6]}) >>> df.is_empty() False >>> df.filter(pl.col("foo") > 99).is_empty() True
 
  - is_sorted(
  - *by: [str](https://docs.python.org/3/library/stdtypes.html#str) | [Iterable](https://docs.python.org/3/library/collections.abc.html#collections.abc.Iterable)[[str](https://docs.python.org/3/library/stdtypes.html#str)]* ,
  - **more_by: [str](https://docs.python.org/3/library/stdtypes.html#str)* ,
  - *descending: [bool](https://docs.python.org/3/library/functions.html#bool) | [Sequence](https://docs.python.org/3/library/collections.abc.html#collections.abc.Sequence)[[bool](https://docs.python.org/3/library/functions.html#bool)] = False* ,
  - *nulls_last: [bool](https://docs.python.org/3/library/functions.html#bool) | [Sequence](https://docs.python.org/3/library/collections.abc.html#collections.abc.Sequence)[[bool](https://docs.python.org/3/library/functions.html#bool)] = False* ,
  - Check whether the DataFrame is sorted by the given columns. 
    - Parameters:
      - **by**
      - Column name(s) to check.
      - ***more_by**
      - Additional column names.
      - **descending**
      - Sort in descending order. When sorting by multiple columns, can be specified per column by passing a sequence of booleans.
      - **nulls_last**
      - Place null values last. When sorting by multiple columns, can be specified per column by passing a sequence of booleans.
 Examples >>> df = pl.DataFrame({"a": [1, 2, 3], "b": [5, 4, 3]}) >>> df.is_sorted("a") True >>> df.is_sorted("b", descending=True) True >>> df.is_sorted("a", "b") True
 [bool](https://docs.python.org/3/library/functions.html#bool)[\[source\]](https://github.com/pola-rs/polars/blob/py-1.43.2/py-polars/src/../src/polars/dataframe/frame.py#L10292-L10336)  - 
is_unique()  Series[\[source\]](https://github.com/pola-rs/polars/blob/py-1.43.2/py-polars/src/../src/polars/dataframe/frame.py#L10375-L10410)
  - Get a mask of all unique rows in this DataFrame. Examples >>> df = pl.DataFrame( ... { ... "a": [1, 2, 3, 1], ... "b": ["x", "y", "z", "x"], ... } ... ) >>> df.is_unique() shape: (4,) Series: '' [bool] [ false true true false ] This mask can be used to visualize the unique lines like this: >>> df.filter(df.is_unique()) shape: (2, 2) ┌─────┬─────┐ │ a ┆ b │ │ --- ┆ --- │ │ i64 ┆ str │ ╞═════╪═════╡ │ 2 ┆ y │ │ 3 ┆ z │ └─────┴─────┘
 
  - 
item(*row: [int](https://docs.python.org/3/library/functions.html#int) | [None](https://docs.python.org/3/library/constants.html#None) = None* ,*column: [int](https://docs.python.org/3/library/functions.html#int) | [str](https://docs.python.org/3/library/stdtypes.html#str) | [None](https://docs.python.org/3/library/constants.html#None) = None* )[Any](https://docs.python.org/3/library/typing.html#typing.Any)[\[source\]](https://github.com/pola-rs/polars/blob/py-1.43.2/py-polars/src/../src/polars/dataframe/frame.py#L1697-L1745)
  - Return the DataFrame as a scalar, or return the element at the given row/column. 
    - Parameters:
      - **row**
      - Optional row index.
      - **column**
      - Optional column index or name.
 See also 
    - [`row`](api/polars.DataFrame.row.html#polars.DataFrame.row)
    - Get the values of a single row, either by index or by predicate.
 Notes If row/col not provided, this is equivalent to `df[0,0]` , with a check that
the shape is (1,1). With row/col, this is equivalent to`df[row,col]` .Examples >>> df = pl.DataFrame({"a": [1, 2, 3], "b": [4, 5, 6]}) >>> df.select((pl.col("a") * pl.col("b")).sum()).item() 32 >>> df.item(1, 1) 5 >>> df.item(2, "b") 6
 
  - 
iter_columns()  Iterator[Series][\[source\]](https://github.com/pola-rs/polars/blob/py-1.43.2/py-polars/src/../src/polars/dataframe/frame.py#L12207-L12261)
  - Returns an iterator over the columns of this DataFrame. 
    - Yields:
      - Series
 Notes Consider whether you can use [`all()`](../expressions/api/polars.all.html#polars.all) instead.
If you can, it will be more efficient.Examples >>> df = pl.DataFrame( ... { ... "a": [1, 3, 5], ... "b": [2, 4, 6], ... } ... ) >>> [s.name for s in df.iter_columns()] ['a', 'b'] If you’re using this to modify a dataframe’s columns, e.g. >>> # Do NOT do this >>> pl.DataFrame(column * 2 for column in df.iter_columns()) shape: (3, 2) ┌─────┬─────┐ │ a ┆ b │ │ --- ┆ --- │ │ i64 ┆ i64 │ ╞═════╪═════╡ │ 2 ┆ 4 │ │ 6 ┆ 8 │ │ 10 ┆ 12 │ └─────┴─────┘ then consider whether you can use [`all()`](../expressions/api/polars.all.html#polars.all) instead:>>> df.select(pl.all() * 2) shape: (3, 2) ┌─────┬─────┐ │ a ┆ b │ │ --- ┆ --- │ │ i64 ┆ i64 │ ╞═════╪═════╡ │ 2 ┆ 4 │ │ 6 ┆ 8 │ │ 10 ┆ 12 │ └─────┴─────┘
 
  - iter_rows( ) Iterator[
  - Returns an iterator over the DataFrame of rows of python-native values. 
    - Parameters:
      - **named**
      - Return dictionaries instead of tuples. The dictionaries are a mapping of column name to row value. This is more expensive than returning a regular tuple, but allows for accessing values by column name.
      - **buffer_size**
      - Determines the number of rows that are buffered internally while iterating over the data; you should only modify this in very specific cases where the default value is determined not to be a good fit to your access pattern, as the speedup from using the buffer is significant (~2-4x). Setting this value to zero disables row buffering (not recommended).
    - Yields:
      - iterator of tuples (default) or dictionaries (if named) of python row values
 Warning Row iteration is not optimal as the underlying data is stored in columnar form; where possible, prefer export via one of the dedicated export/output methods that deals with columnar data. See also 
    - [`rows`](api/polars.DataFrame.rows.html#polars.DataFrame.rows)
    - Materialises all frame data as a list of rows (potentially expensive).
    - [`rows_by_key`](api/polars.DataFrame.rows_by_key.html#polars.DataFrame.rows_by_key)
    - Materialises frame data as a key-indexed dictionary.
 Notes If you have `ns` -precision temporal values you should be aware that Python
natively only supports up to`μs` -precision;`ns` -precision values will be
truncated to microseconds on conversion to Python. If this matters to your
use-case you should export to a different format (such as Arrow or NumPy).Examples >>> df = pl.DataFrame( ... { ... "a": [1, 3, 5], ... "b": [2, 4, 6], ... } ... ) >>> [row[0] for row in df.iter_rows()] [1, 3, 5] >>> [row["b"] for row in df.iter_rows(named=True)] [2, 4, 6]
 [tuple](https://docs.python.org/3/library/stdtypes.html#tuple) [Any, ...]] | Iterator[[dict](https://docs.python.org/3/library/stdtypes.html#dict) [[str](https://docs.python.org/3/library/stdtypes.html#str) , Any]][\[source\]](https://github.com/pola-rs/polars/blob/py-1.43.2/py-polars/src/../src/polars/dataframe/frame.py#L12132-L12205)  - 
iter_slices(*n_rows: [int](https://docs.python.org/3/library/functions.html#int) = 10000* )  Iterator[DataFrame][\[source\]](https://github.com/pola-rs/polars/blob/py-1.43.2/py-polars/src/../src/polars/dataframe/frame.py#L12263-L12309)
  - Returns a non-copying iterator of slices over the underlying DataFrame. 
    - Parameters:
      - **n_rows**
      - Determines the number of rows contained in each DataFrame slice.
 See also 
    - [`iter_rows`](api/polars.DataFrame.iter_rows.html#polars.DataFrame.iter_rows)
    - Row iterator over frame data (does not materialise all rows).
    - [`partition_by`](api/polars.DataFrame.partition_by.html#polars.DataFrame.partition_by)
    - Split into multiple DataFrames, partitioned by groups.
 Examples >>> from datetime import date >>> df = pl.DataFrame( ... data={ ... "a": range(17_500), ... "b": date(2023, 1, 1), ... "c": "klmnoopqrstuvwxyz", ... }, ... schema_overrides={"a": pl.Int32}, ... ) >>> for idx, frame in enumerate(df.iter_slices()): ... print(f"{type(frame).__name__}:[{idx}]:{len(frame)}") DataFrame:[0]:10000 DataFrame:[1]:7500 Using `iter_slices` is an efficient way to chunk-iterate over DataFrames and
any supported frame export/conversion types; for example, as RecordBatches:>>> for frame in df.iter_slices(n_rows=15_000): ... record_batch = frame.to_arrow().to_batches()[0] ... print(f"{record_batch.schema}\n<< {len(record_batch)}") a: int32 b: date32[day] c: large_string << 15000 a: int32 b: date32[day] c: large_string << 2500
 
  - join(
  - *other: DataFrame* ,
  - *on: [str](https://docs.python.org/3/library/stdtypes.html#str) | Expr | Sequence[[str](https://docs.python.org/3/library/stdtypes.html#str) | Expr] | [None](https://docs.python.org/3/library/constants.html#None) = None* ,
  - *how: JoinStrategy = 'inner'* ,
  - *** ,
  - *left_on: [str](https://docs.python.org/3/library/stdtypes.html#str) | Expr | Sequence[[str](https://docs.python.org/3/library/stdtypes.html#str) | Expr] | [None](https://docs.python.org/3/library/constants.html#None) = None* ,
  - *right_on: [str](https://docs.python.org/3/library/stdtypes.html#str) | Expr | Sequence[[str](https://docs.python.org/3/library/stdtypes.html#str) | Expr] | [None](https://docs.python.org/3/library/constants.html#None) = None* ,
  - *suffix: [str](https://docs.python.org/3/library/stdtypes.html#str) = '_right'* ,
  - *validate: JoinValidation = 'm:m'* ,
  - *nulls_equal: [bool](https://docs.python.org/3/library/functions.html#bool) = False* ,
  - *coalesce: [bool](https://docs.python.org/3/library/functions.html#bool) | [None](https://docs.python.org/3/library/constants.html#None) = None* ,
  - *maintain_order: MaintainOrderJoin | [None](https://docs.python.org/3/library/constants.html#None) = None* ,
  - *build_side: JoinBuildSide = 'auto'* ,
  - Join in SQL-like fashion. Changed in version 1.24: The `join_nulls` parameter was renamed`nulls_equal` .
    - Parameters:
      - **other**
      - DataFrame to join with.
      - **on**
      - Name(s) of the join columns in both DataFrames. If set, `left_on` and`right_on` should be None. This should not be specified if`how='cross'` .
      - **how** {‘inner’, ‘left’, ‘right’, ‘full’, ‘semi’, ‘anti’, ‘cross’}
      - Join strategy. **inner***(Default)* Returns rows that have matching values in both tables.**left**Returns all rows from the left table, and the matched rows from the right table. **right**Returns all rows from the right table, and the matched rows from the left table. **full**Returns all rows from both tables, joining matching rows and filling non-matches with null values. **cross**Returns the Cartesian product of rows from both tables **semi**Returns rows from the left table that have a match in the right table. Does not return columns from the right table. **anti**Returns rows from the left table that have no match in the right table. Does not return columns from the right table.
      - **left_on**
      - Name(s) of the left join column(s).
      - **right_on**
      - Name(s) of the right join column(s).
      - **suffix**
      - Suffix to append to columns with a duplicate name.
      - **validate: {‘m:m’, ‘m:1’, ‘1:m’, ‘1:1’}**
      - Checks if join is of specified type. **m:m***(Default)* Many-to-many (default). Does not result in checks.**1:1**One-to-one. Checks if join keys are unique in both left and right datasets. **1:m**One-to-many. Checks if join keys are unique in left dataset. **m:1**Many-to-one. Check if join keys are unique in right dataset. Note This is currently not supported by the streaming engine.
      - **nulls_equal**
      - Join on null values. By default null values will never produce matches.
      - **coalesce**
      - Coalescing behavior (merging of join columns). **None***(Default)* Coalesce unless`how='full'` is specified.**True**Always coalesce join columns. **False**Never coalesce join columns. Note Joining on any other expressions than `col` will turn off coalescing.
      - **maintain_order** {‘none’, ‘left’, ‘right’, ‘left_right’, ‘right_left’}
      - Which DataFrame row order to preserve, if any. Do not rely on any observed ordering without explicitly setting this parameter, as your code may break in a future release. Not specifying any ordering can improve performance. **none***(Default)* No specific ordering is desired. The ordering might
differ across Polars versions or even between different runs.**left**Preserves the order of the left DataFrame. **right**Preserves the order of the right DataFrame. **left_right**First preserves the order of the left DataFrame, then the right. **right_left**First preserves the order of the right DataFrame, then the left.
      - **build_side: {‘auto’, ‘prefer_left’, ‘prefer_right’, ‘force_left’, ‘force_right’}**
      - Which side of the join will be used as the build side. This side will be likely be held in memory as a hash table. Note that unless a `force_` variant is chosen, the chosen side might differ across Polars versions or
even between different runs.**auto***(Default)* Let Polars figure out the build side.**prefer_left**Unless there’s a very good reason to believe that the right side is smaller, use the left side. **prefer_right**Unless there’s a very good reason to believe that the left side is smaller, use the right side. **force_left**Always use the left side. **force_right**Always use the right side. Warning This functionality is considered **experimental** . It may be removed or
changed at any point without it being considered a breaking change.
 See also Examples >>> df = pl.DataFrame( ... { ... "foo": [1, 2, 3], ... "bar": [6.0, 7.0, 8.0], ... "ham": ["a", "b", "c"], ... } ... ) >>> other_df = pl.DataFrame( ... { ... "apple": ["x", "y", "z"], ... "ham": ["a", "b", "d"], ... } ... ) >>> df.join(other_df, on="ham") shape: (2, 4) ┌─────┬─────┬─────┬───────┐ │ foo ┆ bar ┆ ham ┆ apple │ │ --- ┆ --- ┆ --- ┆ --- │ │ i64 ┆ f64 ┆ str ┆ str │ ╞═════╪═════╪═════╪═══════╡ │ 1 ┆ 6.0 ┆ a ┆ x │ │ 2 ┆ 7.0 ┆ b ┆ y │ └─────┴─────┴─────┴───────┘ >>> df.join(other_df, on="ham", how="full") shape: (4, 5) ┌──────┬──────┬──────┬───────┬───────────┐ │ foo ┆ bar ┆ ham ┆ apple ┆ ham_right │ │ --- ┆ --- ┆ --- ┆ --- ┆ --- │ │ i64 ┆ f64 ┆ str ┆ str ┆ str │ ╞══════╪══════╪══════╪═══════╪═══════════╡ │ 1 ┆ 6.0 ┆ a ┆ x ┆ a │ │ 2 ┆ 7.0 ┆ b ┆ y ┆ b │ │ null ┆ null ┆ null ┆ z ┆ d │ │ 3 ┆ 8.0 ┆ c ┆ null ┆ null │ └──────┴──────┴──────┴───────┴───────────┘ >>> df.join(other_df, on="ham", how="full", coalesce=True) shape: (4, 4) ┌──────┬──────┬─────┬───────┐ │ foo ┆ bar ┆ ham ┆ apple │ │ --- ┆ --- ┆ --- ┆ --- │ │ i64 ┆ f64 ┆ str ┆ str │ ╞══════╪══════╪═════╪═══════╡ │ 1 ┆ 6.0 ┆ a ┆ x │ │ 2 ┆ 7.0 ┆ b ┆ y │ │ null ┆ null ┆ d ┆ z │ │ 3 ┆ 8.0 ┆ c ┆ null │ └──────┴──────┴─────┴───────┘ >>> df.join(other_df, on="ham", how="left") shape: (3, 4) ┌─────┬─────┬─────┬───────┐ │ foo ┆ bar ┆ ham ┆ apple │ │ --- ┆ --- ┆ --- ┆ --- │ │ i64 ┆ f64 ┆ str ┆ str │ ╞═════╪═════╪═════╪═══════╡ │ 1 ┆ 6.0 ┆ a ┆ x │ │ 2 ┆ 7.0 ┆ b ┆ y │ │ 3 ┆ 8.0 ┆ c ┆ null │ └─────┴─────┴─────┴───────┘ >>> df.join(other_df, on="ham", how="semi") shape: (2, 3) ┌─────┬─────┬─────┐ │ foo ┆ bar ┆ ham │ │ --- ┆ --- ┆ --- │ │ i64 ┆ f64 ┆ str │ ╞═════╪═════╪═════╡ │ 1 ┆ 6.0 ┆ a │ │ 2 ┆ 7.0 ┆ b │ └─────┴─────┴─────┘ >>> df.join(other_df, on="ham", how="anti") shape: (1, 3) ┌─────┬─────┬─────┐ │ foo ┆ bar ┆ ham │ │ --- ┆ --- ┆ --- │ │ i64 ┆ f64 ┆ str │ ╞═════╪═════╪═════╡ │ 3 ┆ 8.0 ┆ c │ └─────┴─────┴─────┘ >>> df.join(other_df, how="cross") shape: (9, 5) ┌─────┬─────┬─────┬───────┬───────────┐ │ foo ┆ bar ┆ ham ┆ apple ┆ ham_right │ │ --- ┆ --- ┆ --- ┆ --- ┆ --- │ │ i64 ┆ f64 ┆ str ┆ str ┆ str │ ╞═════╪═════╪═════╪═══════╪═══════════╡ │ 1 ┆ 6.0 ┆ a ┆ x ┆ a │ │ 1 ┆ 6.0 ┆ a ┆ y ┆ b │ │ 1 ┆ 6.0 ┆ a ┆ z ┆ d │ │ 2 ┆ 7.0 ┆ b ┆ x ┆ a │ │ 2 ┆ 7.0 ┆ b ┆ y ┆ b │ │ 2 ┆ 7.0 ┆ b ┆ z ┆ d │ │ 3 ┆ 8.0 ┆ c ┆ x ┆ a │ │ 3 ┆ 8.0 ┆ c ┆ y ┆ b │ │ 3 ┆ 8.0 ┆ c ┆ z ┆ d │ └─────┴─────┴─────┴───────┴───────────┘
 [\[source\]](https://github.com/pola-rs/polars/blob/py-1.43.2/py-polars/src/../src/polars/dataframe/frame.py#L8250-L8522)  - join_asof(
  - *other: DataFrame* ,
  - *** ,
  - *left_on: [str](https://docs.python.org/3/library/stdtypes.html#str) | [None](https://docs.python.org/3/library/constants.html#None) | Expr = None* ,
  - *right_on: [str](https://docs.python.org/3/library/stdtypes.html#str) | [None](https://docs.python.org/3/library/constants.html#None) | Expr = None* ,
  - *on: [str](https://docs.python.org/3/library/stdtypes.html#str) | [None](https://docs.python.org/3/library/constants.html#None) | Expr = None* ,
  - *by_left: [str](https://docs.python.org/3/library/stdtypes.html#str) | Sequence[[str](https://docs.python.org/3/library/stdtypes.html#str)] | [None](https://docs.python.org/3/library/constants.html#None) = None* ,
  - *by_right: [str](https://docs.python.org/3/library/stdtypes.html#str) | Sequence[[str](https://docs.python.org/3/library/stdtypes.html#str)] | [None](https://docs.python.org/3/library/constants.html#None) = None* ,
  - *by: [str](https://docs.python.org/3/library/stdtypes.html#str) | Sequence[[str](https://docs.python.org/3/library/stdtypes.html#str)] | [None](https://docs.python.org/3/library/constants.html#None) = None* ,
  - *strategy: AsofJoinStrategy = 'backward'* ,
  - *suffix: [str](https://docs.python.org/3/library/stdtypes.html#str) = '_right'* ,
  - *tolerance: [str](https://docs.python.org/3/library/stdtypes.html#str) | [int](https://docs.python.org/3/library/functions.html#int) | [float](https://docs.python.org/3/library/functions.html#float) | timedelta | [None](https://docs.python.org/3/library/constants.html#None) = None* ,
  - *allow_parallel: [bool](https://docs.python.org/3/library/functions.html#bool) = True* ,
  - *force_parallel: [bool](https://docs.python.org/3/library/functions.html#bool) = False* ,
  - *coalesce: [bool](https://docs.python.org/3/library/functions.html#bool) = True* ,
  - *allow_exact_matches: [bool](https://docs.python.org/3/library/functions.html#bool) = True* ,
  - *check_sortedness: [bool](https://docs.python.org/3/library/functions.html#bool) = True* ,
  - Perform an asof join. This is similar to a left-join except that we match on nearest key rather than equal keys. Both DataFrames must be sorted by the `on` key (within each`by` group, if
specified).For each row in the left DataFrame: 
    - A “backward” search selects the last row in the right DataFrame whose ‘on’ key is less than or equal to the left’s key.
    - A “forward” search selects the first row in the right DataFrame whose ‘on’ key is greater than or equal to the left’s key.
    - A “nearest” search selects the last row in the right DataFrame whose value is nearest to the left’s key. String keys are not currently supported for a nearest search.
 The default is “backward”. 
    - Parameters:
      - **other**
      - Lazy DataFrame to join with.
      - **left_on**
      - Join column of the left DataFrame.
      - **right_on**
      - Join column of the right DataFrame.
      - **on**
      - Join column of both DataFrames. If set, `left_on` and`right_on` should be
None.
      - **by_left**
      - Join on these columns before doing asof join
      - **by_right**
      - Join on these columns before doing asof join
      - **by**
      - Join on these columns before doing asof join
      - **strategy** {‘backward’, ‘forward’, ‘nearest’}
      - Join strategy.
      - **suffix**
      - Suffix to append to columns with a duplicate name.
      - **tolerance**
      - Numeric tolerance. By setting this the join will only be done if the near keys are within this distance. If an asof join is done on columns of dtype “Date”, “Datetime”, “Duration” or “Time”, use either a datetime.timedelta object or the following string language: 
        - 1ns (1 nanosecond)
        - 1us (1 microsecond)
        - 1ms (1 millisecond)
        - 1s (1 second)
        - 1m (1 minute)
        - 1h (1 hour)
        - 1d (1 calendar day)
        - 1w (1 calendar week)
        - 1mo (1 calendar month)
        - 1q (1 calendar quarter)
        - 1y (1 calendar year)
 Or combine them: “3d12h4m25s” # 3 days, 12 hours, 4 minutes, and 25 seconds By “calendar day”, we mean the corresponding time on the next day (which may not be 24 hours, due to daylight savings - in cases of ambiguity, we follow RFC-5545 and preserve the DST fold of the original datetime). Similarly for “calendar week”, “calendar month”, “calendar quarter”, and “calendar year”.
      - **allow_parallel**
      - Allow the physical plan to optionally evaluate the computation of both DataFrames up to the join in parallel.
      - **force_parallel**
      - Force the physical plan to evaluate the computation of both DataFrames up to the join in parallel.
      - **coalesce**
      - Coalescing behavior (merging of `on` /`left_on` /`right_on` columns):
        - *True* : Always coalesce join columns.
        - *False* : Never coalesce join columns.
 Note that joining on any other expressions than `col` will turn off coalescing.
      - **allow_exact_matches**
      - Whether exact matches are valid join predicates. 
          - If True, allow matching with the same `on` value
          - (i.e. less-than-or-equal-to / greater-than-or-equal-to)
        - If True, allow matching with the same 
          - If False, don’t match the same `on` value
          - (i.e., strictly less-than / strictly greater-than).
        - If False, don’t match the same 
      - **check_sortedness**
      - Check the sortedness of the asof keys. If the keys are not sorted Polars will error. Currently, the `in-memory` engine cannot check the sortedness
if ‘by’ groups are provided. The`streaming` engine will only check the
sortedness of the rows it processes.
 Examples >>> from datetime import date >>> gdp = pl.DataFrame( ... { ... "date": pl.date_range( ... date(2016, 1, 1), ... date(2020, 1, 1), ... "1y", ... eager=True, ... ), ... "gdp": [4164, 4411, 4566, 4696, 4827], ... } ... ) >>> gdp shape: (5, 2) ┌────────────┬──────┐ │ date ┆ gdp │ │ --- ┆ --- │ │ date ┆ i64 │ ╞════════════╪══════╡ │ 2016-01-01 ┆ 4164 │ │ 2017-01-01 ┆ 4411 │ │ 2018-01-01 ┆ 4566 │ │ 2019-01-01 ┆ 4696 │ │ 2020-01-01 ┆ 4827 │ └────────────┴──────┘ >>> population = pl.DataFrame( ... { ... "date": [date(2016, 3, 1), date(2018, 8, 1), date(2019, 1, 1)], ... "population": [82.19, 82.66, 83.12], ... } ... ).sort("date") >>> population shape: (3, 2) ┌────────────┬────────────┐ │ date ┆ population │ │ --- ┆ --- │ │ date ┆ f64 │ ╞════════════╪════════════╡ │ 2016-03-01 ┆ 82.19 │ │ 2018-08-01 ┆ 82.66 │ │ 2019-01-01 ┆ 83.12 │ └────────────┴────────────┘ Note how the dates don’t quite match. If we join them using `join_asof` and`strategy='backward'` , then each date from`population` which doesn’t have an
exact match is matched with the closest earlier date from`gdp` :>>> population.join_asof(gdp, on="date", strategy="backward") shape: (3, 3) ┌────────────┬────────────┬──────┐ │ date ┆ population ┆ gdp │ │ --- ┆ --- ┆ --- │ │ date ┆ f64 ┆ i64 │ ╞════════════╪════════════╪══════╡ │ 2016-03-01 ┆ 82.19 ┆ 4164 │ │ 2018-08-01 ┆ 82.66 ┆ 4566 │ │ 2019-01-01 ┆ 83.12 ┆ 4696 │ └────────────┴────────────┴──────┘ Note how: 
    - date `2016-03-01` from`population` is matched with`2016-01-01` from`gdp` ;
    - date `2018-08-01` from`population` is matched with`2018-01-01` from`gdp` .
 You can verify this by passing `coalesce=False` :>>> population.join_asof(gdp, on="date", strategy="backward", coalesce=False) shape: (3, 4) ┌────────────┬────────────┬────────────┬──────┐ │ date ┆ population ┆ date_right ┆ gdp │ │ --- ┆ --- ┆ --- ┆ --- │ │ date ┆ f64 ┆ date ┆ i64 │ ╞════════════╪════════════╪════════════╪══════╡ │ 2016-03-01 ┆ 82.19 ┆ 2016-01-01 ┆ 4164 │ │ 2018-08-01 ┆ 82.66 ┆ 2018-01-01 ┆ 4566 │ │ 2019-01-01 ┆ 83.12 ┆ 2019-01-01 ┆ 4696 │ └────────────┴────────────┴────────────┴──────┘ If we instead use `strategy='forward'` , then each date from`population` which
doesn’t have an exact match is matched with the closest later date from`gdp` :>>> population.join_asof(gdp, on="date", strategy="forward") shape: (3, 3) ┌────────────┬────────────┬──────┐ │ date ┆ population ┆ gdp │ │ --- ┆ --- ┆ --- │ │ date ┆ f64 ┆ i64 │ ╞════════════╪════════════╪══════╡ │ 2016-03-01 ┆ 82.19 ┆ 4411 │ │ 2018-08-01 ┆ 82.66 ┆ 4696 │ │ 2019-01-01 ┆ 83.12 ┆ 4696 │ └────────────┴────────────┴──────┘ Note how: 
    - date `2016-03-01` from`population` is matched with`2017-01-01` from`gdp` ;
    - date `2018-08-01` from`population` is matched with`2019-01-01` from`gdp` .
 Finally, `strategy='nearest'` gives us a mix of the two results above, as each
date from`population` which doesn’t have an exact match is matched with the
closest date from`gdp` , regardless of whether it’s earlier or later:>>> population.join_asof(gdp, on="date", strategy="nearest") shape: (3, 3) ┌────────────┬────────────┬──────┐ │ date ┆ population ┆ gdp │ │ --- ┆ --- ┆ --- │ │ date ┆ f64 ┆ i64 │ ╞════════════╪════════════╪══════╡ │ 2016-03-01 ┆ 82.19 ┆ 4164 │ │ 2018-08-01 ┆ 82.66 ┆ 4696 │ │ 2019-01-01 ┆ 83.12 ┆ 4696 │ └────────────┴────────────┴──────┘ Note how: 
    - date `2016-03-01` from`population` is matched with`2016-01-01` from`gdp` ;
    - date `2018-08-01` from`population` is matched with`2019-01-01` from`gdp` .
 The `by` argument allows joining on another column first, before the asof join.
In this example we join by`country` first, then asof join by date, as above.>>> gdp_dates = pl.date_range( # fmt: skip ... date(2016, 1, 1), date(2020, 1, 1), "1y", eager=True ... ) >>> gdp2 = pl.DataFrame( ... { ... "country": ["Germany"] * 5 + ["Netherlands"] * 5, ... "date": pl.concat([gdp_dates, gdp_dates]), ... "gdp": [4164, 4411, 4566, 4696, 4827, 784, 833, 914, 910, 909], ... } ... ).sort("country", "date") >>> >>> gdp2 shape: (10, 3) ┌─────────────┬────────────┬──────┐ │ country ┆ date ┆ gdp │ │ --- ┆ --- ┆ --- │ │ str ┆ date ┆ i64 │ ╞═════════════╪════════════╪══════╡ │ Germany ┆ 2016-01-01 ┆ 4164 │ │ Germany ┆ 2017-01-01 ┆ 4411 │ │ Germany ┆ 2018-01-01 ┆ 4566 │ │ Germany ┆ 2019-01-01 ┆ 4696 │ │ Germany ┆ 2020-01-01 ┆ 4827 │ │ Netherlands ┆ 2016-01-01 ┆ 784 │ │ Netherlands ┆ 2017-01-01 ┆ 833 │ │ Netherlands ┆ 2018-01-01 ┆ 914 │ │ Netherlands ┆ 2019-01-01 ┆ 910 │ │ Netherlands ┆ 2020-01-01 ┆ 909 │ └─────────────┴────────────┴──────┘ >>> pop2 = pl.DataFrame( ... { ... "country": ["Germany"] * 3 + ["Netherlands"] * 3, ... "date": [ ... date(2016, 3, 1), ... date(2018, 8, 1), ... date(2019, 1, 1), ... date(2016, 3, 1), ... date(2018, 8, 1), ... date(2019, 1, 1), ... ], ... "population": [82.19, 82.66, 83.12, 17.11, 17.32, 17.40], ... } ... ).sort("country", "date") >>> >>> pop2 shape: (6, 3) ┌─────────────┬────────────┬────────────┐ │ country ┆ date ┆ population │ │ --- ┆ --- ┆ --- │ │ str ┆ date ┆ f64 │ ╞═════════════╪════════════╪════════════╡ │ Germany ┆ 2016-03-01 ┆ 82.19 │ │ Germany ┆ 2018-08-01 ┆ 82.66 │ │ Germany ┆ 2019-01-01 ┆ 83.12 │ │ Netherlands ┆ 2016-03-01 ┆ 17.11 │ │ Netherlands ┆ 2018-08-01 ┆ 17.32 │ │ Netherlands ┆ 2019-01-01 ┆ 17.4 │ └─────────────┴────────────┴────────────┘ >>> pop2.join_asof(gdp2, by="country", on="date", strategy="nearest") shape: (6, 4) ┌─────────────┬────────────┬────────────┬──────┐ │ country ┆ date ┆ population ┆ gdp │ │ --- ┆ --- ┆ --- ┆ --- │ │ str ┆ date ┆ f64 ┆ i64 │ ╞═════════════╪════════════╪════════════╪══════╡ │ Germany ┆ 2016-03-01 ┆ 82.19 ┆ 4164 │ │ Germany ┆ 2018-08-01 ┆ 82.66 ┆ 4696 │ │ Germany ┆ 2019-01-01 ┆ 83.12 ┆ 4696 │ │ Netherlands ┆ 2016-03-01 ┆ 17.11 ┆ 784 │ │ Netherlands ┆ 2018-08-01 ┆ 17.32 ┆ 910 │ │ Netherlands ┆ 2019-01-01 ┆ 17.4 ┆ 910 │ └─────────────┴────────────┴────────────┴──────┘
 [\[source\]](https://github.com/pola-rs/polars/blob/py-1.43.2/py-polars/src/../src/polars/dataframe/frame.py#L7895-L8248)  - join_where(
  - *other: DataFrame* ,
  - **predicates: Expr | Iterable[Expr]* ,
  - *suffix: [str](https://docs.python.org/3/library/stdtypes.html#str) = '_right'* ,
  - Perform a join based on one or multiple (in)equality predicates. This performs an inner join, so only rows where all predicates are true are included in the result, and a row from either DataFrame may be included multiple times in the result. Note The row order of the input DataFrames is not preserved. Warning This functionality is experimental. It may be changed at any point without it being considered a breaking change. 
    - Parameters:
      - **other**
      - DataFrame to join with.
      - ***predicates**
      - (In)Equality condition to join the two tables on. When a column name occurs in both tables, the proper suffix must be applied in the predicate.
      - **suffix**
      - Suffix to append to columns with a duplicate name.
 Examples Join two dataframes together based on two predicates which get AND-ed together. >>> east = pl.DataFrame( ... { ... "id": [100, 101, 102], ... "dur": [120, 140, 160], ... "rev": [12, 14, 16], ... "cores": [2, 8, 4], ... } ... ) >>> west = pl.DataFrame( ... { ... "t_id": [404, 498, 676, 742], ... "time": [90, 130, 150, 170], ... "cost": [9, 13, 15, 16], ... "cores": [4, 2, 1, 4], ... } ... ) >>> east.join_where( ... west, ... pl.col("dur") < pl.col("time"), ... pl.col("rev") < pl.col("cost"), ... ) shape: (5, 8) ┌─────┬─────┬─────┬───────┬──────┬──────┬──────┬─────────────┐ │ id ┆ dur ┆ rev ┆ cores ┆ t_id ┆ time ┆ cost ┆ cores_right │ │ --- ┆ --- ┆ --- ┆ --- ┆ --- ┆ --- ┆ --- ┆ --- │ │ i64 ┆ i64 ┆ i64 ┆ i64 ┆ i64 ┆ i64 ┆ i64 ┆ i64 │ ╞═════╪═════╪═════╪═══════╪══════╪══════╪══════╪═════════════╡ │ 100 ┆ 120 ┆ 12 ┆ 2 ┆ 498 ┆ 130 ┆ 13 ┆ 2 │ │ 100 ┆ 120 ┆ 12 ┆ 2 ┆ 676 ┆ 150 ┆ 15 ┆ 1 │ │ 100 ┆ 120 ┆ 12 ┆ 2 ┆ 742 ┆ 170 ┆ 16 ┆ 4 │ │ 101 ┆ 140 ┆ 14 ┆ 8 ┆ 676 ┆ 150 ┆ 15 ┆ 1 │ │ 101 ┆ 140 ┆ 14 ┆ 8 ┆ 742 ┆ 170 ┆ 16 ┆ 4 │ └─────┴─────┴─────┴───────┴──────┴──────┴──────┴─────────────┘ To OR them together, use a single expression and the `|` operator.>>> east.join_where( ... west, ... (pl.col("dur") < pl.col("time")) | (pl.col("rev") < pl.col("cost")), ... ) shape: (6, 8) ┌─────┬─────┬─────┬───────┬──────┬──────┬──────┬─────────────┐ │ id ┆ dur ┆ rev ┆ cores ┆ t_id ┆ time ┆ cost ┆ cores_right │ │ --- ┆ --- ┆ --- ┆ --- ┆ --- ┆ --- ┆ --- ┆ --- │ │ i64 ┆ i64 ┆ i64 ┆ i64 ┆ i64 ┆ i64 ┆ i64 ┆ i64 │ ╞═════╪═════╪═════╪═══════╪══════╪══════╪══════╪═════════════╡ │ 100 ┆ 120 ┆ 12 ┆ 2 ┆ 498 ┆ 130 ┆ 13 ┆ 2 │ │ 100 ┆ 120 ┆ 12 ┆ 2 ┆ 676 ┆ 150 ┆ 15 ┆ 1 │ │ 100 ┆ 120 ┆ 12 ┆ 2 ┆ 742 ┆ 170 ┆ 16 ┆ 4 │ │ 101 ┆ 140 ┆ 14 ┆ 8 ┆ 676 ┆ 150 ┆ 15 ┆ 1 │ │ 101 ┆ 140 ┆ 14 ┆ 8 ┆ 742 ┆ 170 ┆ 16 ┆ 4 │ │ 102 ┆ 160 ┆ 16 ┆ 4 ┆ 742 ┆ 170 ┆ 16 ┆ 4 │ └─────┴─────┴─────┴───────┴──────┴──────┴──────┴─────────────┘
 [\[source\]](https://github.com/pola-rs/polars/blob/py-1.43.2/py-polars/src/../src/polars/dataframe/frame.py#L8524-L8626)  - 
lazy()  LazyFrame[\[source\]](https://github.com/pola-rs/polars/blob/py-1.43.2/py-polars/src/../src/polars/dataframe/frame.py#L10412-L10447)
  - Start a lazy query from this point. This returns a `LazyFrame` object.Operations on a `LazyFrame` are not executed until this is triggered
by calling one of:
      - [`.collect()`](../lazyframe/api/polars.LazyFrame.collect.html#polars.LazyFrame.collect)
      - (run on all data)
      - [`.explain()`](../lazyframe/api/polars.LazyFrame.explain.html#polars.LazyFrame.explain)
      - (print the query plan)
      - [`.show_graph()`](../lazyframe/api/polars.LazyFrame.show_graph.html#polars.LazyFrame.show_graph)
      - (show the query plan as graphviz graph)
      - [`.collect_schema()`](../lazyframe/api/polars.LazyFrame.collect_schema.html#polars.LazyFrame.collect_schema)
      - (return the final frame schema)
 Lazy operations are recommended because they allow for query optimization and additional parallelism. 
    - Returns:
      - LazyFrame
 Examples >>> df = pl.DataFrame( ... { ... "a": [None, 2, 3, 4], ... "b": [0.5, None, 2.5, 13], ... "c": [True, True, False, None], ... } ... ) >>> df.lazy() <LazyFrame at ...>
 
  - 
limit(*n: [int](https://docs.python.org/3/library/functions.html#int) = 5* )  DataFrame[\[source\]](https://github.com/pola-rs/polars/blob/py-1.43.2/py-polars/src/../src/polars/dataframe/frame.py#L6638-L6677)
  - Get the first `n` rows.Alias for [`DataFrame.head()`](api/polars.DataFrame.head.html#polars.DataFrame.head) .
    - Parameters:
      - **n**
      - Number of rows to return. If a negative value is passed, return all rows except the last `abs(n)` .
 See also Examples Get the first 3 rows of a DataFrame. >>> df = pl.DataFrame( ... { ... "foo": [1, 2, 3, 4, 5], ... "bar": [6, 7, 8, 9, 10], ... "ham": ["a", "b", "c", "d", "e"], ... } ... ) >>> df.limit(3) shape: (3, 3) ┌─────┬─────┬─────┐ │ foo ┆ bar ┆ ham │ │ --- ┆ --- ┆ --- │ │ i64 ┆ i64 ┆ str │ ╞═════╪═════╪═════╡ │ 1 ┆ 6 ┆ a │ │ 2 ┆ 7 ┆ b │ │ 3 ┆ 8 ┆ c │ └─────┴─────┴─────┘
 
  - map_columns(
  - *column_names: [str](https://docs.python.org/3/library/stdtypes.html#str) | Sequence[[str](https://docs.python.org/3/library/stdtypes.html#str)] | [Selector](../selectors.html#polars.selectors.Selector)* ,
  - *function: Callable[Concatenate[Series, P], Series]* ,
  - **args: P.args* ,
  - ***kwargs: P.kwargs* ,
  - Apply eager functions to columns of a DataFrame. Users should always prefer [`with_columns()`](api/polars.DataFrame.with_columns.html#polars.DataFrame.with_columns) unless they are using
expressions that are only possible on`Series` and not on`Expr` . This is almost
never the case, except for a very select few functions that cannot know the
output datatype without looking at the data.
    - Parameters:
      - **column_names**
      - The columns to apply the UDF to.
      - **function**
      - Callable; will receive a column series as the first parameter, followed by any given args/kwargs.
      - ***args**
      - Arguments to pass to the UDF.
      - ****kwargs**
      - Keyword arguments to pass to the UDF.
 See also Examples >>> df = pl.DataFrame({"a": [1, 2, 3, 4], "b": ["10", "20", "30", "40"]}) >>> df.map_columns("a", lambda s: s.shrink_dtype()) shape: (4, 2) ┌─────┬─────┐ │ a ┆ b │ │ --- ┆ --- │ │ i8 ┆ str │ ╞═════╪═════╡ │ 1 ┆ 10 │ │ 2 ┆ 20 │ │ 3 ┆ 30 │ │ 4 ┆ 40 │ └─────┴─────┘ >>> df = pl.DataFrame( ... { ... "a": ['{"x":"a"}', None, '{"x":"b"}', None], ... "b": ['{"a":1, "b": true}', None, '{"a":2, "b": false}', None], ... } ... ) >>> df.map_columns(["a", "b"], lambda s: s.str.json_decode()) shape: (4, 2) ┌───────────┬───────────┐ │ a ┆ b │ │ --- ┆ --- │ │ struct[1] ┆ struct[2] │ ╞═══════════╪═══════════╡ │ {"a"} ┆ {1,true} │ │ null ┆ null │ │ {"b"} ┆ {2,false} │ │ null ┆ null │ └───────────┴───────────┘ >>> import polars.selectors as cs >>> df.map_columns(cs.all(), lambda s: s.str.json_decode()) shape: (4, 2) ┌───────────┬───────────┐ │ a ┆ b │ │ --- ┆ --- │ │ struct[1] ┆ struct[2] │ ╞═══════════╪═══════════╡ │ {"a"} ┆ {1,true} │ │ null ┆ null │ │ {"b"} ┆ {2,false} │ │ null ┆ null │ └───────────┴───────────┘
 [\[source\]](https://github.com/pola-rs/polars/blob/py-1.43.2/py-polars/src/../src/polars/dataframe/frame.py#L6957-L7048)  - map_rows(
  - *function: Callable[[[tuple](https://docs.python.org/3/library/stdtypes.html#tuple)[Any, ...]], Any]* ,
  - *return_dtype: PolarsDataType | [None](https://docs.python.org/3/library/constants.html#None) = None* ,
  - *** ,
  - *inference_size: [int](https://docs.python.org/3/library/functions.html#int) = 256* ,
  - Apply a custom/user-defined function (UDF) over the rows of the DataFrame. Warning This method is much slower than the native expressions API. Only use it if you cannot implement your logic otherwise. The UDF will receive each row as a tuple of values: `udf(row)` .Implementing logic using a Python function is almost always *significantly* slower and more memory intensive than implementing the same logic using
the native expression API because:
    - The native expression engine runs in Rust; UDFs run in Python.
    - Use of Python UDFs forces the DataFrame to be materialized in memory.
    - Polars-native expressions can be parallelised (UDFs typically cannot).
    - Polars-native expressions can be logically optimised (UDFs cannot).
 Wherever possible you should strongly prefer the native expression API to achieve the best performance. 
    - Parameters:
      - **function**
      - Custom function or lambda.
      - **return_dtype**
      - Output type of the operation. If none given, Polars tries to infer the type.
      - **inference_size**
      - Only used in the case when the custom function returns rows. This uses the first `n` rows to determine the output schema.
 Notes 
    - The frame-level `map_rows` cannot track column names (as the UDF is a
black-box that may arbitrarily drop, rearrange, transform, or add new
columns); if you want to apply a UDF such that column names are preserved,
you should use the expression-level`map_elements` syntax instead.
    - If your function is expensive and you don’t want it to be called more than once for a given input, consider applying an `@lru_cache` decorator to it.
If your data is suitable you may achieve*significant* speedups.
 Examples >>> df = pl.DataFrame({"foo": [1, 2, 3], "bar": [-1, 5, 8]}) Return a DataFrame by mapping each row to a tuple: >>> df.map_rows(lambda t: (t[0] * 2, t[1] * 3)) shape: (3, 2) ┌──────────┬──────────┐ │ column_0 ┆ column_1 │ │ --- ┆ --- │ │ i64 ┆ i64 │ ╞══════════╪══════════╡ │ 2 ┆ -3 │ │ 4 ┆ 15 │ │ 6 ┆ 24 │ └──────────┴──────────┘ However, it is much better to implement this with a native expression: >>> df.select( ... pl.col("foo") * 2, ... pl.col("bar") * 3, ... ) Return a DataFrame with a single column by mapping each row to a scalar: >>> df.map_rows(lambda t: t[0] * 2 + t[1]) shape: (3, 1) ┌─────┐ │ map │ │ --- │ │ i64 │ ╞═════╡ │ 1 │ │ 9 │ │ 14 │ └─────┘ In this case it is better to use the following native expression: >>> df.select(pl.col("foo") * 2 + pl.col("bar"))
 [\[source\]](https://github.com/pola-rs/polars/blob/py-1.43.2/py-polars/src/../src/polars/dataframe/frame.py#L8686-L8786)  - match_to_schema(
  - *schema: SchemaDict | Schema* ,
  - *** ,
  - *missing_columns: Literal['insert', 'raise'] | Mapping[[str](https://docs.python.org/3/library/stdtypes.html#str), Literal['insert', 'raise'] | Expr] = 'raise'* ,
  - *missing_struct_fields: Literal['insert', 'raise'] | Mapping[[str](https://docs.python.org/3/library/stdtypes.html#str), Literal['insert', 'raise']] = 'raise'* ,
  - *extra_columns: Literal['ignore', 'raise'] = 'raise'* ,
  - *extra_struct_fields: Literal['ignore', 'raise'] | Mapping[[str](https://docs.python.org/3/library/stdtypes.html#str), Literal['ignore', 'raise']] = 'raise'* ,
  - *integer_cast: Literal['upcast', 'forbid'] | Mapping[[str](https://docs.python.org/3/library/stdtypes.html#str), Literal['upcast', 'forbid']] = 'forbid'* ,
  - *float_cast: Literal['upcast', 'forbid'] | Mapping[[str](https://docs.python.org/3/library/stdtypes.html#str), Literal['upcast', 'forbid']] = 'forbid'* ,
  - Match or evolve the schema of a LazyFrame into a specific schema. By default, match_to_schema returns an error if the input schema does not exactly match the target schema. It also allows columns to be freely reordered, with additional coercion rules available through optional parameters. Warning This functionality is considered **unstable** . It may be changed
at any point without it being considered a breaking change.
    - Parameters:
      - **schema**
      - Target schema to match or evolve to.
      - **missing_columns**
      - Raise of insert missing columns from the input with respect to the `schema` .This can also be an expression per column with what to insert if it is missing.
      - **missing_struct_fields**
      - Raise of insert missing struct fields from the input with respect to the `schema` .
      - **extra_columns**
      - Raise of ignore extra columns from the input with respect to the `schema` .
      - **extra_struct_fields**
      - Raise of ignore extra struct fields from the input with respect to the `schema` .
      - **integer_cast**
      - Forbid of upcast for integer columns from the input to the respective column in `schema` .
      - **float_cast**
      - Forbid of upcast for float columns from the input to the respective column in `schema` .
 Examples Ensuring the schema matches >>> df = pl.DataFrame({"a": [1, 2, 3], "b": ["A", "B", "C"]}) >>> df.match_to_schema({"a": pl.Int64, "b": pl.String}) shape: (3, 2) ┌─────┬─────┐ │ a ┆ b │ │ --- ┆ --- │ │ i64 ┆ str │ ╞═════╪═════╡ │ 1 ┆ A │ │ 2 ┆ B │ │ 3 ┆ C │ └─────┴─────┘ >>> df.match_to_schema({"a": pl.Int64}) polars.exceptions.SchemaError: extra columns in `match_to_schema`: "b" Adding missing columns >>> ( ... pl.DataFrame({"a": [1, 2, 3]}).match_to_schema( ... {"a": pl.Int64, "b": pl.String}, ... missing_columns="insert", ... ) ... ) shape: (3, 2) ┌─────┬──────┐ │ a ┆ b │ │ --- ┆ --- │ │ i64 ┆ str │ ╞═════╪══════╡ │ 1 ┆ null │ │ 2 ┆ null │ │ 3 ┆ null │ └─────┴──────┘ >>> ( ... pl.DataFrame({"a": [1, 2, 3]}).match_to_schema( ... {"a": pl.Int64, "b": pl.String}, ... missing_columns={"b": pl.col.a.cast(pl.String)}, ... ) ... ) shape: (3, 2) ┌─────┬─────┐ │ a ┆ b │ │ --- ┆ --- │ │ i64 ┆ str │ ╞═════╪═════╡ │ 1 ┆ 1 │ │ 2 ┆ 2 │ │ 3 ┆ 3 │ └─────┴─────┘ Removing extra columns >>> ( ... pl.DataFrame({"a": [1, 2, 3], "b": ["A", "B", "C"]}).match_to_schema( ... {"a": pl.Int64}, ... extra_columns="ignore", ... ) ... ) shape: (3, 1) ┌─────┐ │ a │ │ --- │ │ i64 │ ╞═════╡ │ 1 │ │ 2 │ │ 3 │ └─────┘ Upcasting integers and floats >>> ( ... pl.DataFrame( ... {"a": [1, 2, 3], "b": [1.0, 2.0, 3.0]}, ... schema={"a": pl.Int32, "b": pl.Float32}, ... ).match_to_schema( ... {"a": pl.Int64, "b": pl.Float64}, ... integer_cast="upcast", ... float_cast="upcast", ... ) ... ) shape: (3, 2) ┌─────┬─────┐ │ a ┆ b │ │ --- ┆ --- │ │ i64 ┆ f64 │ ╞═════╪═════╡ │ 1 ┆ 1.0 │ │ 2 ┆ 2.0 │ │ 3 ┆ 3.0 │ └─────┴─────┘
 [\[source\]](https://github.com/pola-rs/polars/blob/py-1.43.2/py-polars/src/../src/polars/dataframe/frame.py#L13234-L13396)  - 
max()  DataFrame[\[source\]](https://github.com/pola-rs/polars/blob/py-1.43.2/py-polars/src/../src/polars/dataframe/frame.py#L10787-L10812)
  - Aggregate the columns of this DataFrame to their maximum value. Examples >>> df = pl.DataFrame( ... { ... "foo": [1, 2, 3], ... "bar": [6, 7, 8], ... "ham": ["a", "b", "c"], ... } ... ) >>> df.max() shape: (1, 3) ┌─────┬─────┬─────┐ │ foo ┆ bar ┆ ham │ │ --- ┆ --- ┆ --- │ │ i64 ┆ i64 ┆ str │ ╞═════╪═════╪═════╡ │ 3 ┆ 8 ┆ c │ └─────┴─────┴─────┘
 
  - 
max_horizontal()  Series[\[source\]](https://github.com/pola-rs/polars/blob/py-1.43.2/py-polars/src/../src/polars/dataframe/frame.py#L10814-L10840)
  - Get the maximum value horizontally across columns. 
    - Returns:
      - Series
      - A Series named `"max"` .
 Examples >>> df = pl.DataFrame( ... { ... "foo": [1, 2, 3], ... "bar": [4.0, 5.0, 6.0], ... } ... ) >>> df.max_horizontal() shape: (3,) Series: 'max' [f64] [ 4.0 5.0 6.0 ]
 
  - 
mean()  DataFrame[\[source\]](https://github.com/pola-rs/polars/blob/py-1.43.2/py-polars/src/../src/polars/dataframe/frame.py#L10960-L10986)
  - Aggregate the columns of this DataFrame to their mean value. Examples >>> df = pl.DataFrame( ... { ... "foo": [1, 2, 3], ... "bar": [6, 7, 8], ... "ham": ["a", "b", "c"], ... "spam": [True, False, None], ... } ... ) >>> df.mean() shape: (1, 4) ┌─────┬─────┬──────┬──────┐ │ foo ┆ bar ┆ ham ┆ spam │ │ --- ┆ --- ┆ --- ┆ --- │ │ f64 ┆ f64 ┆ str ┆ f64 │ ╞═════╪═════╪══════╪══════╡ │ 2.0 ┆ 7.0 ┆ null ┆ 0.5 │ └─────┴─────┴──────┴──────┘
 
  - 
mean_horizontal(*** ,*ignore_nulls: [bool](https://docs.python.org/3/library/functions.html#bool) = True* )  Series[\[source\]](https://github.com/pola-rs/polars/blob/py-1.43.2/py-polars/src/../src/polars/dataframe/frame.py#L10988-L11022)
  - Take the mean of all values horizontally across columns. 
    - Parameters:
      - **ignore_nulls**
      - Ignore null values (default). If set to `False` , any null value in the input will lead to a null output.
    - Returns:
      - Series
      - A Series named `"mean"` .
 Examples >>> df = pl.DataFrame( ... { ... "foo": [1, 2, 3], ... "bar": [4.0, 5.0, 6.0], ... } ... ) >>> df.mean_horizontal() shape: (3,) Series: 'mean' [f64] [ 2.5 3.5 4.5 ]
 
  - 
median()  DataFrame[\[source\]](https://github.com/pola-rs/polars/blob/py-1.43.2/py-polars/src/../src/polars/dataframe/frame.py#L11110-L11135)
  - Aggregate the columns of this DataFrame to their median value. Examples >>> df = pl.DataFrame( ... { ... "foo": [1, 2, 3], ... "bar": [6, 7, 8], ... "ham": ["a", "b", "c"], ... } ... ) >>> df.median() shape: (1, 3) ┌─────┬─────┬──────┐ │ foo ┆ bar ┆ ham │ │ --- ┆ --- ┆ --- │ │ f64 ┆ f64 ┆ str │ ╞═════╪═════╪══════╡ │ 2.0 ┆ 7.0 ┆ null │ └─────┴─────┴──────┘
 
  - melt(
  - *id_vars: ColumnNameOrSelector | Sequence[ColumnNameOrSelector] | [None](https://docs.python.org/3/library/constants.html#None) = None* ,
  - *value_vars: ColumnNameOrSelector | Sequence[ColumnNameOrSelector] | [None](https://docs.python.org/3/library/constants.html#None) = None* ,
  - *variable_name: [str](https://docs.python.org/3/library/stdtypes.html#str) | [None](https://docs.python.org/3/library/constants.html#None) = None* ,
  - *value_name: [str](https://docs.python.org/3/library/stdtypes.html#str) | [None](https://docs.python.org/3/library/constants.html#None) = None* ,
  - Unpivot a DataFrame from wide to long format. Optionally leaves identifiers set. This function is useful to massage a DataFrame into a format where one or more columns are identifier variables (id_vars) while all other columns, considered measured variables (value_vars), are “unpivoted” to the row axis leaving just two non-identifier columns, ‘variable’ and ‘value’. Deprecated since version 1.0.0: Use the [`unpivot()`](api/polars.DataFrame.unpivot.html#polars.DataFrame.unpivot) method instead.
    - Parameters:
      - **id_vars**
      - Column(s) or selector(s) to use as identifier variables.
      - **value_vars**
      - Column(s) or selector(s) to use as values variables; if `value_vars` is empty all columns that are not in`id_vars` will be used.
      - **variable_name**
      - Name to give to the `variable` column. Defaults to “variable”
      - **value_name**
      - Name to give to the `value` column. Defaults to “value”
 [\[source\]](https://github.com/pola-rs/polars/blob/py-1.43.2/py-polars/src/../src/polars/dataframe/frame.py#L12981-L13022)  - merge_sorted( ) DataFrame
  - Take two sorted DataFrames and merge them by the sorted key. The output of this operation will also be sorted. It is the callers responsibility that the frames are sorted in ascending order by the key(s), with null keys at the start, otherwise the order of the output will not make sense. The schemas of both DataFrames must be equal. 
    - Parameters:
      - **other**
      - Other DataFrame that must be merged
      - **key**
      - Key column(s) that the frames are sorted by. A single column name or a sequence of column names can be passed. When multiple keys are given the frames are merged as if sorted by those keys in order.
      - **maintain_order**
      - If `True` , the output is guaranteed to have left-biased ordering
for equal keys: rows from the left frame appear before rows from
the right frame when their keys are equal.
 Notes Unless `maintain_order=True` , no guarantee is given over the output
row order when the key is equal between the both dataframes.The key(s) must be sorted in ascending order. Examples >>> df0 = pl.DataFrame( ... {"name": ["steve", "elise", "bob"], "age": [42, 44, 18]} ... ).sort("age") >>> df0 shape: (3, 2) ┌───────┬─────┐ │ name ┆ age │ │ --- ┆ --- │ │ str ┆ i64 │ ╞═══════╪═════╡ │ bob ┆ 18 │ │ steve ┆ 42 │ │ elise ┆ 44 │ └───────┴─────┘ >>> df1 = pl.DataFrame( ... {"name": ["anna", "megan", "steve", "thomas"], "age": [21, 33, 42, 20]} ... ).sort("age") >>> df1 shape: (4, 2) ┌────────┬─────┐ │ name ┆ age │ │ --- ┆ --- │ │ str ┆ i64 │ ╞════════╪═════╡ │ thomas ┆ 20 │ │ anna ┆ 21 │ │ megan ┆ 33 │ │ steve ┆ 42 │ └────────┴─────┘ >>> df0.merge_sorted(df1, key="age") shape: (7, 2) ┌────────┬─────┐ │ name ┆ age │ │ --- ┆ --- │ │ str ┆ i64 │ ╞════════╪═════╡ │ bob ┆ 18 │ │ thomas ┆ 20 │ │ anna ┆ 21 │ │ megan ┆ 33 │ │ steve ┆ 42 │ │ steve ┆ 42 │ │ elise ┆ 44 │ └────────┴─────┘ Multiple keys can be passed to merge frames sorted by a composite key. The frames are merged as if sorted by `key_1` , then`key_2` .>>> df0 = pl.DataFrame({"key_1": [1, 1, 3], "key_2": [1, 4, 2]}) >>> df1 = pl.DataFrame({"key_1": [1, 2, 3], "key_2": [2, 1, 1]}) >>> df0.merge_sorted(df1, key=["key_1", "key_2"]) shape: (6, 2) ┌───────┬───────┐ │ key_1 ┆ key_2 │ │ --- ┆ --- │ │ i64 ┆ i64 │ ╞═══════╪═══════╡ │ 1 ┆ 1 │ │ 1 ┆ 2 │ │ 1 ┆ 4 │ │ 2 ┆ 1 │ │ 3 ┆ 1 │ │ 3 ┆ 2 │ └───────┴───────┘
 [\[source\]](https://github.com/pola-rs/polars/blob/py-1.43.2/py-polars/src/../src/polars/dataframe/frame.py#L12648-L12761)  - 
min()  DataFrame[\[source\]](https://github.com/pola-rs/polars/blob/py-1.43.2/py-polars/src/../src/polars/dataframe/frame.py#L10842-L10867)
  - Aggregate the columns of this DataFrame to their minimum value. Examples >>> df = pl.DataFrame( ... { ... "foo": [1, 2, 3], ... "bar": [6, 7, 8], ... "ham": ["a", "b", "c"], ... } ... ) >>> df.min() shape: (1, 3) ┌─────┬─────┬─────┐ │ foo ┆ bar ┆ ham │ │ --- ┆ --- ┆ --- │ │ i64 ┆ i64 ┆ str │ ╞═════╪═════╪═════╡ │ 1 ┆ 6 ┆ a │ └─────┴─────┴─────┘
 
  - 
min_horizontal()  Series[\[source\]](https://github.com/pola-rs/polars/blob/py-1.43.2/py-polars/src/../src/polars/dataframe/frame.py#L10869-L10895)
  - Get the minimum value horizontally across columns. 
    - Returns:
      - Series
      - A Series named `"min"` .
 Examples >>> df = pl.DataFrame( ... { ... "foo": [1, 2, 3], ... "bar": [4.0, 5.0, 6.0], ... } ... ) >>> df.min_horizontal() shape: (3,) Series: 'min' [f64] [ 1.0 2.0 3.0 ]
 
  - 
n_chunks(*strategy: Literal['first', 'all'] = 'first'* )[int](https://docs.python.org/3/library/functions.html#int) |[list](https://docs.python.org/3/library/stdtypes.html#list) [[int](https://docs.python.org/3/library/functions.html#int) ][\[source\]](https://github.com/pola-rs/polars/blob/py-1.43.2/py-polars/src/../src/polars/dataframe/frame.py#L10751-L10785)
  - Get number of chunks used by the ChunkedArrays of this DataFrame. 
    - Parameters:
      - **strategy** {‘first’, ‘all’}
      - Return the number of chunks of the ‘first’ column, or ‘all’ columns in this DataFrame.
 Examples >>> df = pl.DataFrame( ... { ... "a": [1, 2, 3, 4], ... "b": [0.5, 4, 10, 13], ... "c": [True, True, False, True], ... } ... ) >>> df.n_chunks() 1 >>> df.n_chunks(strategy="all") [1, 1, 1]
 
  - 
n_unique(*subset: [str](https://docs.python.org/3/library/stdtypes.html#str) | Expr | Sequence[[str](https://docs.python.org/3/library/stdtypes.html#str) | Expr] | [None](https://docs.python.org/3/library/constants.html#None) = None* )[int](https://docs.python.org/3/library/functions.html#int)[\[source\]](https://github.com/pola-rs/polars/blob/py-1.43.2/py-polars/src/../src/polars/dataframe/frame.py#L11438-L11512)
  - Return the number of unique rows, or the number of unique row-subsets. 
    - Parameters:
      - **subset**
      - One or more columns/expressions that define what to count; omit to return the count of unique rows.
 Notes This method operates at the `DataFrame` level; to operate on subsets at the
expression level you can make use of struct-packing instead, for example:>>> expr_unique_subset = pl.struct("a", "b").n_unique() If instead you want to count the number of unique values per-column, you can also use expression-level syntax to return a new frame containing that result: >>> df = pl.DataFrame( ... [[1, 2, 3], [1, 2, 4]], schema=["a", "b", "c"], orient="row" ... ) >>> df_nunique = df.select(pl.all().n_unique()) In aggregate context there is also an equivalent method for returning the unique values per-group: >>> df_agg_nunique = df.group_by("a").n_unique() Examples >>> df = pl.DataFrame( ... { ... "a": [1, 1, 2, 3, 4, 5], ... "b": [0.5, 0.5, 1.0, 2.0, 3.0, 3.0], ... "c": [True, True, True, False, True, True], ... } ... ) >>> df.n_unique() 5 Simple columns subset. >>> df.n_unique(subset=["b", "c"]) 4 Expression subset. >>> df.n_unique( ... subset=[ ... (pl.col("a") // 2), ... (pl.col("c") | (pl.col("b") >= 2)), ... ], ... ) 3
 
  - 
null_count()  DataFrame[\[source\]](https://github.com/pola-rs/polars/blob/py-1.43.2/py-polars/src/../src/polars/dataframe/frame.py#L11560-L11583)
  - Create a new DataFrame that shows the null counts per column. Examples >>> df = pl.DataFrame( ... { ... "foo": [1, None, 3], ... "bar": [6, 7, None], ... "ham": ["a", "b", "c"], ... } ... ) >>> df.null_count() shape: (1, 3) ┌─────┬─────┬─────┐ │ foo ┆ bar ┆ ham │ │ --- ┆ --- ┆ --- │ │ u32 ┆ u32 ┆ u32 │ ╞═════╪═════╪═════╡ │ 1 ┆ 1 ┆ 0 │ └─────┴─────┴─────┘
 
  - partition_by(
  - *by: ColumnNameOrSelector | Sequence[ColumnNameOrSelector]* ,
  - **more_by: ColumnNameOrSelector* ,
  - *maintain_order: [bool](https://docs.python.org/3/library/functions.html#bool) = True* ,
  - *include_key: [bool](https://docs.python.org/3/library/functions.html#bool) = True* ,
  - *as_dict: [bool](https://docs.python.org/3/library/functions.html#bool) = False* ,
  - Group by the given columns and return the groups as separate dataframes. 
    - Parameters:
      - **by**
      - Column name(s) or selector(s) to group by.
      - ***more_by**
      - Additional names of columns to group by, specified as positional arguments.
      - **maintain_order**
      - Ensure that the order of the groups is consistent with the input data. This is slower than a default partition by operation.
      - **include_key**
      - Include the columns used to partition the DataFrame in the output.
      - **as_dict**
      - Return a dictionary instead of a list. The dictionary keys are tuples of the distinct group values that identify each group.
 Examples Pass a single column name to partition by that column. >>> df = pl.DataFrame( ... { ... "a": ["a", "b", "a", "b", "c"], ... "b": [1, 2, 1, 3, 3], ... "c": [5, 4, 3, 2, 1], ... } ... ) >>> df.partition_by("a") [shape: (2, 3) ┌─────┬─────┬─────┐ │ a ┆ b ┆ c │ │ --- ┆ --- ┆ --- │ │ str ┆ i64 ┆ i64 │ ╞═════╪═════╪═════╡ │ a ┆ 1 ┆ 5 │ │ a ┆ 1 ┆ 3 │ └─────┴─────┴─────┘, shape: (2, 3) ┌─────┬─────┬─────┐ │ a ┆ b ┆ c │ │ --- ┆ --- ┆ --- │ │ str ┆ i64 ┆ i64 │ ╞═════╪═════╪═════╡ │ b ┆ 2 ┆ 4 │ │ b ┆ 3 ┆ 2 │ └─────┴─────┴─────┘, shape: (1, 3) ┌─────┬─────┬─────┐ │ a ┆ b ┆ c │ │ --- ┆ --- ┆ --- │ │ str ┆ i64 ┆ i64 │ ╞═════╪═════╪═════╡ │ c ┆ 3 ┆ 1 │ └─────┴─────┴─────┘] Partition by multiple columns by either passing a list of column names, or by specifying each column name as a positional argument. >>> df.partition_by("a", "b") [shape: (2, 3) ┌─────┬─────┬─────┐ │ a ┆ b ┆ c │ │ --- ┆ --- ┆ --- │ │ str ┆ i64 ┆ i64 │ ╞═════╪═════╪═════╡ │ a ┆ 1 ┆ 5 │ │ a ┆ 1 ┆ 3 │ └─────┴─────┴─────┘, shape: (1, 3) ┌─────┬─────┬─────┐ │ a ┆ b ┆ c │ │ --- ┆ --- ┆ --- │ │ str ┆ i64 ┆ i64 │ ╞═════╪═════╪═════╡ │ b ┆ 2 ┆ 4 │ └─────┴─────┴─────┘, shape: (1, 3) ┌─────┬─────┬─────┐ │ a ┆ b ┆ c │ │ --- ┆ --- ┆ --- │ │ str ┆ i64 ┆ i64 │ ╞═════╪═════╪═════╡ │ b ┆ 3 ┆ 2 │ └─────┴─────┴─────┘, shape: (1, 3) ┌─────┬─────┬─────┐ │ a ┆ b ┆ c │ │ --- ┆ --- ┆ --- │ │ str ┆ i64 ┆ i64 │ ╞═════╪═════╪═════╡ │ c ┆ 3 ┆ 1 │ └─────┴─────┴─────┘] Return the partitions as a dictionary by specifying `as_dict=True` .>>> import polars.selectors as cs >>> df.partition_by(cs.string(), as_dict=True) {('a',): shape: (2, 3) ┌─────┬─────┬─────┐ │ a ┆ b ┆ c │ │ --- ┆ --- ┆ --- │ │ str ┆ i64 ┆ i64 │ ╞═════╪═════╪═════╡ │ a ┆ 1 ┆ 5 │ │ a ┆ 1 ┆ 3 │ └─────┴─────┴─────┘, ('b',): shape: (2, 3) ┌─────┬─────┬─────┐ │ a ┆ b ┆ c │ │ --- ┆ --- ┆ --- │ │ str ┆ i64 ┆ i64 │ ╞═════╪═════╪═════╡ │ b ┆ 2 ┆ 4 │ │ b ┆ 3 ┆ 2 │ └─────┴─────┴─────┘, ('c',): shape: (1, 3) ┌─────┬─────┬─────┐ │ a ┆ b ┆ c │ │ --- ┆ --- ┆ --- │ │ str ┆ i64 ┆ i64 │ ╞═════╪═════╪═════╡ │ c ┆ 3 ┆ 1 │ └─────┴─────┴─────┘}
 [list](https://docs.python.org/3/library/stdtypes.html#list) [DataFrame] |[dict](https://docs.python.org/3/library/stdtypes.html#dict) [[tuple](https://docs.python.org/3/library/stdtypes.html#tuple) [Any, ...], DataFrame][\[source\]](https://github.com/pola-rs/polars/blob/py-1.43.2/py-polars/src/../src/polars/dataframe/frame.py#L10059-L10211)  - pipe(
  - *function: Callable[Concatenate[DataFrame, P], T]* ,
  - **args: P.args* ,
  - ***kwargs: P.kwargs* ,
  - Offers a structured way to apply a sequence of user-defined functions (UDFs). 
    - Parameters:
      - **function**
      - Callable; will receive the frame as the first parameter, followed by any given args/kwargs.
      - ***args**
      - Arguments to pass to the UDF.
      - ****kwargs**
      - Keyword arguments to pass to the UDF.
 Notes It is recommended to use LazyFrame when piping operations, in order to fully take advantage of query optimization and parallelization. See [`df.lazy()`](api/polars.DataFrame.lazy.html#polars.DataFrame.lazy) .Examples >>> def cast_str_to_int(data, col_name): ... return data.with_columns(pl.col(col_name).cast(pl.Int64)) >>> df = pl.DataFrame({"a": [1, 2, 3, 4], "b": ["10", "20", "30", "40"]}) >>> df.pipe(cast_str_to_int, col_name="b") shape: (4, 2) ┌─────┬─────┐ │ a ┆ b │ │ --- ┆ --- │ │ i64 ┆ i64 │ ╞═════╪═════╡ │ 1 ┆ 10 │ │ 2 ┆ 20 │ │ 3 ┆ 30 │ │ 4 ┆ 40 │ └─────┴─────┘ >>> df = pl.DataFrame({"b": [1, 2], "a": [3, 4]}) >>> df shape: (2, 2) ┌─────┬─────┐ │ b ┆ a │ │ --- ┆ --- │ │ i64 ┆ i64 │ ╞═════╪═════╡ │ 1 ┆ 3 │ │ 2 ┆ 4 │ └─────┴─────┘ >>> df.pipe(lambda tdf: tdf.select(sorted(tdf.columns))) shape: (2, 2) ┌─────┬─────┐ │ a ┆ b │ │ --- ┆ --- │ │ i64 ┆ i64 │ ╞═════╪═════╡ │ 3 ┆ 1 │ │ 4 ┆ 2 │ └─────┴─────┘
 [\[source\]](https://github.com/pola-rs/polars/blob/py-1.43.2/py-polars/src/../src/polars/dataframe/frame.py#L6890-L6955)  - pivot(
  - *on: ColumnNameOrSelector | Sequence[ColumnNameOrSelector]* ,
  - *on_columns: Sequence[Any] | Series | DataFrame | [None](https://docs.python.org/3/library/constants.html#None) = None* ,
  - *** ,
  - *index: ColumnNameOrSelector | Sequence[ColumnNameOrSelector] | [None](https://docs.python.org/3/library/constants.html#None) = None* ,
  - *values: ColumnNameOrSelector | Sequence[ColumnNameOrSelector] | [None](https://docs.python.org/3/library/constants.html#None) = None* ,
  - *aggregate_function: PivotAgg | Expr | [None](https://docs.python.org/3/library/constants.html#None) = None* ,
  - *maintain_order: [bool](https://docs.python.org/3/library/functions.html#bool) = True* ,
  - *sort_columns: [bool](https://docs.python.org/3/library/functions.html#bool) = False* ,
  - *separator: [str](https://docs.python.org/3/library/stdtypes.html#str) = '_'* ,
  - *column_naming: Literal['auto', 'combine'] = 'auto'* ,
  - Create a spreadsheet-style pivot table as a DataFrame. Only available in eager mode. See “Examples” section below for how to do a “lazy pivot” if you know the unique column values in advance. Changed in version 1.0.0: The `columns` parameter was renamed`on` .
    - Parameters:
      - **on**
      - The column(s) whose values will be used as the new columns of the output DataFrame.
      - **on_columns**
      - What value combinations will be considered for the output table.
      - **index**
      - The column(s) that remain from the input to the output. The output DataFrame will have one row for each unique combination of the `index` ’s values.
If None, all remaining columns not specified on`on` and`values` will be used. At least one
of`index` and`values` must be specified.
      - **values**
      - The existing column(s) of values which will be moved under the new columns from index. If an aggregation is specified, these are the values on which the aggregation will be computed. If None, all remaining columns not specified on `on` and`index` will be used.
At least one of`index` and`values` must be specified.
      - **aggregate_function**
      - Choose from: 
        - None: no aggregation takes place, will raise error if multiple values are in group.
        - A predefined aggregate function string, one of {‘min’, ‘max’, ‘first’, ‘last’, ‘sum’, ‘mean’, ‘median’, ‘len’}
        - An expression to do the aggregation. The expression can only access data from the respective ‘values’ columns as generated by pivot, through `pl.element()` .
      - **maintain_order**
      - Ensure the values of `index` are sorted by discovery order.
      - **sort_columns**
      - Sort the transposed columns by name. Default is by order of discovery.
      - **separator**
      - Used as separator/delimiter in generated column names in case of multiple `values` columns.
      - **column_naming** {‘auto’, ‘combine’}
      - How resulting column names will be constructed. 
          - ‘auto’: The default; combine with separator if there are multiple
          - `values` columns, otherwise just use the`on_columns` names.
          - ‘combine’: Always combine the `values` columns’ names with
          - the `on_columns` names.
        - ‘combine’: Always combine the 
 Warning This functionality is considered **unstable** . It may be changed
at any point without it being considered a breaking change.
    - Returns:
      - DataFrame
 See also Notes In some other frameworks, you might know this operation as `pivot_wider` .Examples You can use `pivot` to reshape a dataframe from “long” to “wide” format.For example, suppose we have a dataframe of test scores achieved by some students, where each row represents a distinct test. >>> df = pl.DataFrame( ... { ... "name": ["Cady", "Cady", "Karen", "Karen"], ... "subject": ["maths", "physics", "maths", "physics"], ... "test_1": [98, 99, 61, 58], ... "test_2": [100, 100, 60, 60], ... } ... ) >>> df shape: (4, 4) ┌───────┬─────────┬────────┬────────┐ │ name ┆ subject ┆ test_1 ┆ test_2 │ │ --- ┆ --- ┆ --- ┆ --- │ │ str ┆ str ┆ i64 ┆ i64 │ ╞═══════╪═════════╪════════╪════════╡ │ Cady ┆ maths ┆ 98 ┆ 100 │ │ Cady ┆ physics ┆ 99 ┆ 100 │ │ Karen ┆ maths ┆ 61 ┆ 60 │ │ Karen ┆ physics ┆ 58 ┆ 60 │ └───────┴─────────┴────────┴────────┘ Using `pivot` , we can reshape so we have one row per student, with different
subjects as columns, and their`test_1` scores as values:>>> df.pivot("subject", index="name", values="test_1") shape: (2, 3) ┌───────┬───────┬─────────┐ │ name ┆ maths ┆ physics │ │ --- ┆ --- ┆ --- │ │ str ┆ i64 ┆ i64 │ ╞═══════╪═══════╪═════════╡ │ Cady ┆ 98 ┆ 99 │ │ Karen ┆ 61 ┆ 58 │ └───────┴───────┴─────────┘ If you want to only pivot over a limited set of `subject` values or already
know the`subject` values ahead of time, you can provide these using the`on_columns` argument.>>> df.pivot( ... "subject", ... on_columns=["maths", "physics"], ... index="name", ... values="test_1", ... ) shape: (2, 3) ┌───────┬───────┬─────────┐ │ name ┆ maths ┆ physics │ │ --- ┆ --- ┆ --- │ │ str ┆ i64 ┆ i64 │ ╞═══════╪═══════╪═════════╡ │ Cady ┆ 98 ┆ 99 │ │ Karen ┆ 61 ┆ 58 │ └───────┴───────┴─────────┘ You can use selectors too - here we include all test scores in the pivoted table: >>> import polars.selectors as cs >>> df.pivot("subject", values=cs.starts_with("test")) shape: (2, 5) ┌───────┬──────────────┬────────────────┬──────────────┬────────────────┐ │ name ┆ test_1_maths ┆ test_1_physics ┆ test_2_maths ┆ test_2_physics │ │ --- ┆ --- ┆ --- ┆ --- ┆ --- │ │ str ┆ i64 ┆ i64 ┆ i64 ┆ i64 │ ╞═══════╪══════════════╪════════════════╪══════════════╪════════════════╡ │ Cady ┆ 98 ┆ 99 ┆ 100 ┆ 100 │ │ Karen ┆ 61 ┆ 58 ┆ 60 ┆ 60 │ └───────┴──────────────┴────────────────┴──────────────┴────────────────┘ If you end up with multiple values per cell, you can specify how to aggregate them with `aggregate_function` :>>> df = pl.DataFrame( ... { ... "ix": [1, 1, 2, 2, 1, 2], ... "col": ["a", "a", "a", "a", "b", "b"], ... "foo": [0, 1, 2, 2, 7, 1], ... "bar": [0, 2, 0, 0, 9, 4], ... } ... ) >>> df.pivot("col", index="ix", aggregate_function="sum") shape: (2, 5) ┌─────┬───────┬───────┬───────┬───────┐ │ ix ┆ foo_a ┆ foo_b ┆ bar_a ┆ bar_b │ │ --- ┆ --- ┆ --- ┆ --- ┆ --- │ │ i64 ┆ i64 ┆ i64 ┆ i64 ┆ i64 │ ╞═════╪═══════╪═══════╪═══════╪═══════╡ │ 1 ┆ 1 ┆ 7 ┆ 2 ┆ 9 │ │ 2 ┆ 4 ┆ 1 ┆ 0 ┆ 4 │ └─────┴───────┴───────┴───────┴───────┘ You can also pass a custom aggregation function using [`polars.element()`](../expressions/api/polars.element.html#polars.element) :>>> df = pl.DataFrame( ... { ... "col1": ["a", "a", "a", "b", "b", "b"], ... "col2": ["x", "x", "x", "x", "y", "y"], ... "col3": [6, 7, 3, 2, 5, 7], ... } ... ) >>> df.pivot( ... "col2", ... index="col1", ... values="col3", ... aggregate_function=pl.element().tanh().mean(), ... ) shape: (2, 3) ┌──────┬──────────┬──────────┐ │ col1 ┆ x ┆ y │ │ --- ┆ --- ┆ --- │ │ str ┆ f64 ┆ f64 │ ╞══════╪══════════╪══════════╡ │ a ┆ 0.998347 ┆ null │ │ b ┆ 0.964028 ┆ 0.999954 │ └──────┴──────────┴──────────┘
 [\[source\]](https://github.com/pola-rs/polars/blob/py-1.43.2/py-polars/src/../src/polars/dataframe/frame.py#L9595-L9822)  - 
*property* plot*: DataFramePlot*[\[source\]](https://github.com/pola-rs/polars/blob/py-1.43.2/py-polars/src/../src/polars/dataframe/frame.py#L672-L767)
  - Create a plot namespace. Warning This functionality is currently considered **unstable** . It may be
changed at any point without it being considered a breaking change.Changed in version 1.6.0: In prior versions of Polars, HvPlot was the plotting backend. If you would like to restore the previous plotting functionality, all you need to do is add `import hvplot.polars` at the top of your script and replace`df.plot` with`df.hvplot` .Polars does not implement plotting logic itself, but instead defers to [Altair](https://altair-viz.github.io/) :
    - `df.plot.line(**kwargs)` is shorthand for`alt.Chart(df).mark_line(tooltip=True).encode(**kwargs).interactive()`
    - `df.plot.point(**kwargs)` is shorthand for`alt.Chart(df).mark_point(tooltip=True).encode(**kwargs).interactive()` (and`plot.scatter` is provided as an alias)
    - `df.plot.bar(**kwargs)` is shorthand for`alt.Chart(df).mark_bar(tooltip=True).encode(**kwargs).interactive()`
    - for any other attribute `attr` ,`df.plot.attr(**kwargs)` is shorthand for`alt.Chart(df).mark_attr(tooltip=True).encode(**kwargs).interactive()`
 For configuration, we suggest reading [Chart Configuration](https://altair-viz.github.io/altair-tutorial/notebooks/08-Configuration.html) .
For example, you can:
    - Change the width/height/title with `.properties(width=500, height=350, title="My amazing plot")` .
    - Change the x-axis label rotation with `.configure_axisX(labelAngle=30)` .
    - Change the opacity of the points in your scatter plot with `.configure_point(opacity=.5)` .
 Examples Scatter plot: >>> df = pl.DataFrame( ... { ... "length": [1, 4, 6], ... "width": [4, 5, 6], ... "species": ["setosa", "setosa", "versicolor"], ... } ... ) >>> df.plot.point(x="length", y="width", color="species") Set the x-axis title by using `altair.X` :>>> import altair as alt >>> df.plot.point( ... x=alt.X("length", title="Length"), y="width", color="species" ... ) Line plot: >>> from datetime import date >>> df = pl.DataFrame( ... { ... "date": [date(2020, 1, 2), date(2020, 1, 3), date(2020, 1, 4)] * 2, ... "price": [1, 4, 6, 1, 5, 2], ... "stock": ["a", "a", "a", "b", "b", "b"], ... } ... ) >>> df.plot.line(x="date", y="price", color="stock") Bar plot: >>> df = pl.DataFrame( ... { ... "day": ["Mon", "Tue", "Wed", "Thu", "Fri", "Sat", "Sun"] * 2, ... "group": ["a"] * 7 + ["b"] * 7, ... "value": [1, 3, 2, 4, 5, 6, 1, 1, 3, 2, 4, 5, 1, 2], ... } ... ) >>> df.plot.bar( ... x="day", y="value", color="day", column="group" ... ) Or, to make a stacked version of the plot above: >>> df.plot.bar(x="day", y="value", color="group")
 
  - 
product()  DataFrame[\[source\]](https://github.com/pola-rs/polars/blob/py-1.43.2/py-polars/src/../src/polars/dataframe/frame.py#L11137-L11168)
  - Aggregate the columns of this DataFrame to their product values. Examples >>> df = pl.DataFrame( ... { ... "a": [1, 2, 3], ... "b": [0.5, 4, 10], ... "c": [True, True, False], ... } ... ) >>> df.product() shape: (1, 3) ┌─────┬──────┬─────┐ │ a ┆ b ┆ c │ │ --- ┆ --- ┆ --- │ │ i64 ┆ f64 ┆ i64 │ ╞═════╪══════╪═════╡ │ 6 ┆ 20.0 ┆ 0 │ └─────┴──────┴─────┘
 
  - quantile(
  - *quantile: [float](https://docs.python.org/3/library/functions.html#float)* ,
  - *interpolation: QuantileMethod = 'nearest'* ,
  - Aggregate the columns of this DataFrame to their quantile value. 
    - Parameters:
      - **quantile**
      - Quantile between 0.0 and 1.0.
      - **interpolation** {‘nearest’, ‘higher’, ‘lower’, ‘midpoint’, ‘linear’, ‘equiprobable’}
      - Interpolation method.
 Examples >>> df = pl.DataFrame( ... { ... "foo": [1, 2, 3], ... "bar": [6, 7, 8], ... "ham": ["a", "b", "c"], ... } ... ) >>> df.quantile(0.5, "nearest") shape: (1, 3) ┌─────┬─────┬──────┐ │ foo ┆ bar ┆ ham │ │ --- ┆ --- ┆ --- │ │ f64 ┆ f64 ┆ str │ ╞═════╪═════╪══════╡ │ 2.0 ┆ 7.0 ┆ null │ └─────┴─────┴──────┘
 [\[source\]](https://github.com/pola-rs/polars/blob/py-1.43.2/py-polars/src/../src/polars/dataframe/frame.py#L11170-L11208)  - 
rechunk()  DataFrame[\[source\]](https://github.com/pola-rs/polars/blob/py-1.43.2/py-polars/src/../src/polars/dataframe/frame.py#L11551-L11558)
  - Rechunk the data in this DataFrame to a contiguous allocation. This will make sure all subsequent operations have optimal and predictable performance.
 
  - remove(
  - **predicates: IntoExprColumn | Iterable[IntoExprColumn] | [bool](https://docs.python.org/3/library/functions.html#bool) | [list](https://docs.python.org/3/library/stdtypes.html#list)[[bool](https://docs.python.org/3/library/functions.html#bool)] | np.ndarray[Any, Any]* ,
  - ***constraints: Any* ,
  - Remove rows, dropping those that match the given predicate expression(s). The original order of the remaining rows is preserved. Rows where the filter predicate does not evaluate to True are retained (this includes rows where the predicate evaluates as `null` ).
    - Parameters:
      - **predicates**
      - Expression(s) that evaluate to a boolean Series. When multiple predicates are provided they are combined using `&` (logical AND), so a row is only
removed when*every* predicate evaluates to True for that row.
      - **constraints**
      - Column filters; use `name = value` to filter columns using the supplied
value. Each constraint behaves the same as`pl.col(name).eq(value)` ,
and is implicitly joined with the other filter conditions using`&` .
 See also Notes If you are transitioning from Pandas, and performing filter operations based on the comparison of two or more columns, please note that in Polars any comparison involving `null` values will result in a`null` result,*not* boolean True or
False. As a result, these rows will not be removed. Ensure that null values
are handled appropriately to avoid unexpected behaviour (see examples below).Examples >>> df = pl.DataFrame( ... { ... "foo": [2, 3, None, 4, 0], ... "bar": [5, 6, None, None, 0], ... "ham": ["a", "b", None, "c", "d"], ... } ... ) Remove rows matching a condition: >>> df.remove(pl.col("bar") >= 5) shape: (3, 3) ┌──────┬──────┬──────┐ │ foo ┆ bar ┆ ham │ │ --- ┆ --- ┆ --- │ │ i64 ┆ i64 ┆ str │ ╞══════╪══════╪══════╡ │ null ┆ null ┆ null │ │ 4 ┆ null ┆ c │ │ 0 ┆ 0 ┆ d │ └──────┴──────┴──────┘ Discard rows based on multiple conditions, combined with and/or operators: >>> df.remove( ... (pl.col("foo") >= 0) & (pl.col("bar") >= 0), ... ) shape: (2, 3) ┌──────┬──────┬──────┐ │ foo ┆ bar ┆ ham │ │ --- ┆ --- ┆ --- │ │ i64 ┆ i64 ┆ str │ ╞══════╪══════╪══════╡ │ null ┆ null ┆ null │ │ 4 ┆ null ┆ c │ └──────┴──────┴──────┘ >>> df.remove( ... (pl.col("foo") >= 0) | (pl.col("bar") >= 0), ... ) shape: (1, 3) ┌──────┬──────┬──────┐ │ foo ┆ bar ┆ ham │ │ --- ┆ --- ┆ --- │ │ i64 ┆ i64 ┆ str │ ╞══════╪══════╪══════╡ │ null ┆ null ┆ null │ └──────┴──────┴──────┘ Provide multiple constraints using `*args` syntax:>>> df.remove( ... pl.col("ham").is_not_null(), ... pl.col("bar") >= 0, ... ) shape: (2, 3) ┌──────┬──────┬──────┐ │ foo ┆ bar ┆ ham │ │ --- ┆ --- ┆ --- │ │ i64 ┆ i64 ┆ str │ ╞══════╪══════╪══════╡ │ null ┆ null ┆ null │ │ 4 ┆ null ┆ c │ └──────┴──────┴──────┘ Provide constraints(s) using `**kwargs` syntax:>>> df.remove(foo=0, bar=0) shape: (4, 3) ┌──────┬──────┬──────┐ │ foo ┆ bar ┆ ham │ │ --- ┆ --- ┆ --- │ │ i64 ┆ i64 ┆ str │ ╞══════╪══════╪══════╡ │ 2 ┆ 5 ┆ a │ │ 3 ┆ 6 ┆ b │ │ null ┆ null ┆ null │ │ 4 ┆ null ┆ c │ └──────┴──────┴──────┘ Remove rows by comparing two columns against each other: >>> df.remove( ... pl.col("foo").ne_missing(pl.col("bar")), ... ) shape: (2, 3) ┌──────┬──────┬──────┐ │ foo ┆ bar ┆ ham │ │ --- ┆ --- ┆ --- │ │ i64 ┆ i64 ┆ str │ ╞══════╪══════╪══════╡ │ null ┆ null ┆ null │ │ 0 ┆ 0 ┆ d │ └──────┴──────┴──────┘
 [\[source\]](https://github.com/pola-rs/polars/blob/py-1.43.2/py-polars/src/../src/polars/dataframe/frame.py#L5565-L5710)  - rename( ) DataFrame
  - Rename column names. 
    - Parameters:
      - **mapping**
      - Key value pairs that map from old name to new name, or a function that takes the old name as input and returns the new name.
      - **strict**
      - Validate that all column names exist in the current schema, and throw an exception if any do not. (Note that this parameter is a no-op when passing a function to `mapping` ).
 See also Examples >>> df = pl.DataFrame( ... {"foo": [1, 2, 3], "bar": [6, 7, 8], "ham": ["a", "b", "c"]} ... ) >>> df.rename({"foo": "apple"}) shape: (3, 3) ┌───────┬─────┬─────┐ │ apple ┆ bar ┆ ham │ │ --- ┆ --- ┆ --- │ │ i64 ┆ i64 ┆ str │ ╞═══════╪═════╪═════╡ │ 1 ┆ 6 ┆ a │ │ 2 ┆ 7 ┆ b │ │ 3 ┆ 8 ┆ c │ └───────┴─────┴─────┘ >>> df.rename(lambda column_name: "c" + column_name[1:]) shape: (3, 3) ┌─────┬─────┬─────┐ │ coo ┆ car ┆ cam │ │ --- ┆ --- ┆ --- │ │ i64 ┆ i64 ┆ str │ ╞═════╪═════╪═════╡ │ 1 ┆ 6 ┆ a │ │ 2 ┆ 7 ┆ b │ │ 3 ┆ 8 ┆ c │ └─────┴─────┴─────┘
 [\[source\]](https://github.com/pola-rs/polars/blob/py-1.43.2/py-polars/src/../src/polars/dataframe/frame.py#L5263-L5317)  - 
replace_column(*index: [int](https://docs.python.org/3/library/functions.html#int)* ,*column: Series* )  DataFrame[\[source\]](https://github.com/pola-rs/polars/blob/py-1.43.2/py-polars/src/../src/polars/dataframe/frame.py#L6037-L6075)
  - Replace a column at an index location. This operation is in place. 
    - Parameters:
      - **index**
      - Column index.
      - **column**
      - Series that will replace the column.
 Examples >>> df = pl.DataFrame( ... { ... "foo": [1, 2, 3], ... "bar": [6, 7, 8], ... "ham": ["a", "b", "c"], ... } ... ) >>> s = pl.Series("apple", [10, 20, 30]) >>> df.replace_column(0, s) shape: (3, 3) ┌───────┬─────┬─────┐ │ apple ┆ bar ┆ ham │ │ --- ┆ --- ┆ --- │ │ i64 ┆ i64 ┆ str │ ╞═══════╪═════╪═════╡ │ 10 ┆ 6 ┆ a │ │ 20 ┆ 7 ┆ b │ │ 30 ┆ 8 ┆ c │ └───────┴─────┴─────┘
 
  - 
reverse()  DataFrame[\[source\]](https://github.com/pola-rs/polars/blob/py-1.43.2/py-polars/src/../src/polars/dataframe/frame.py#L5237-L5261)
  - Reverse the DataFrame. Examples >>> df = pl.DataFrame( ... { ... "key": ["a", "b", "c"], ... "val": [1, 2, 3], ... } ... ) >>> df.reverse() shape: (3, 2) ┌─────┬─────┐ │ key ┆ val │ │ --- ┆ --- │ │ str ┆ i64 │ ╞═════╪═════╡ │ c ┆ 3 │ │ b ┆ 2 │ │ a ┆ 1 │ └─────┴─────┘
 
  - rolling(
  - *index_column: IntoExpr* ,
  - *** ,
  - *period: [str](https://docs.python.org/3/library/stdtypes.html#str) | timedelta* ,
  - *offset: [str](https://docs.python.org/3/library/stdtypes.html#str) | timedelta | [None](https://docs.python.org/3/library/constants.html#None) = None* ,
  - *closed: ClosedInterval = 'right'* ,
  - *group_by: IntoExpr | Iterable[IntoExpr] | [None](https://docs.python.org/3/library/constants.html#None) = None* ,
  - Create rolling groups based on a temporal or integer column. Different from a `group_by_dynamic` the windows are now determined by the
individual values and are not of constant intervals. For constant intervals use[`DataFrame.group_by_dynamic()`](api/polars.DataFrame.group_by_dynamic.html#polars.DataFrame.group_by_dynamic) .If you have a time series `<t_0, t_1, ..., t_n>` , then by default the
windows created will be
    - (t_0 - period, t_0]
    - (t_1 - period, t_1]
    - …
    - (t_n - period, t_n]
 whereas if you pass a non-default `offset` , then the windows will be
    - (t_0 + offset, t_0 + offset + period]
    - (t_1 + offset, t_1 + offset + period]
    - …
    - (t_n + offset, t_n + offset + period]
 The `period` and`offset` arguments are created either from a timedelta, or
by using the following string language:
    - 1ns (1 nanosecond)
    - 1us (1 microsecond)
    - 1ms (1 millisecond)
    - 1s (1 second)
    - 1m (1 minute)
    - 1h (1 hour)
    - 1d (1 calendar day)
    - 1w (1 calendar week)
    - 1mo (1 calendar month)
    - 1q (1 calendar quarter)
    - 1y (1 calendar year)
    - 1i (1 index count)
 Or combine them: “3d12h4m25s” # 3 days, 12 hours, 4 minutes, and 25 seconds By “calendar day”, we mean the corresponding time on the next day (which may not be 24 hours, due to daylight savings). Similarly for “calendar week”, “calendar month”, “calendar quarter”, and “calendar year”. Changed in version 0.20.14: The `by` parameter was renamed`group_by` .
    - Parameters:
      - **index_column**
      - Column used to group based on the time window. Often of type Date/Datetime. This column must be sorted in ascending order (or, if `group_by` is
specified, then it must be sorted in ascending order within each group).In case of a rolling operation on indices, dtype needs to be one of {UInt32, UInt64, Int32, Int64}. Note that the first three get temporarily cast to Int64, so if performance matters use an Int64 column.
      - **period**
      - Length of the window - must be non-negative.
      - **offset**
      - Offset of the window. Default is `-period` .
      - **closed** {‘right’, ‘left’, ‘both’, ‘none’}
      - Define which sides of the temporal interval are closed (inclusive).
      - **group_by**
      - Also group by this column/these columns
    - Returns:
      - RollingGroupBy
      - Object you can call `.agg` on to aggregate by groups, the result
of which will be sorted by`index_column` (but note that if`group_by` columns are passed, it will only be sorted within each group).
 See also Examples >>> dates = [ ... "2020-01-01 13:45:48", ... "2020-01-01 16:42:13", ... "2020-01-01 16:45:09", ... "2020-01-02 18:12:48", ... "2020-01-03 19:45:32", ... "2020-01-08 23:16:43", ... ] >>> df = pl.DataFrame({"dt": dates, "a": [3, 7, 5, 9, 2, 1]}).with_columns( ... pl.col("dt").str.strptime(pl.Datetime).set_sorted() ... ) >>> out = df.rolling(index_column="dt", period="2d").agg( ... [ ... pl.sum("a").alias("sum_a"), ... pl.min("a").alias("min_a"), ... pl.max("a").alias("max_a"), ... ] ... ) >>> assert out["sum_a"].to_list() == [3, 10, 15, 24, 11, 1] >>> assert out["max_a"].to_list() == [3, 7, 7, 9, 9, 1] >>> assert out["min_a"].to_list() == [3, 3, 3, 3, 2, 1] >>> out shape: (6, 4) ┌─────────────────────┬───────┬───────┬───────┐ │ dt ┆ sum_a ┆ min_a ┆ max_a │ │ --- ┆ --- ┆ --- ┆ --- │ │ datetime[μs] ┆ i64 ┆ i64 ┆ i64 │ ╞═════════════════════╪═══════╪═══════╪═══════╡ │ 2020-01-01 13:45:48 ┆ 3 ┆ 3 ┆ 3 │ │ 2020-01-01 16:42:13 ┆ 10 ┆ 3 ┆ 7 │ │ 2020-01-01 16:45:09 ┆ 15 ┆ 3 ┆ 7 │ │ 2020-01-02 18:12:48 ┆ 24 ┆ 3 ┆ 9 │ │ 2020-01-03 19:45:32 ┆ 11 ┆ 2 ┆ 9 │ │ 2020-01-08 23:16:43 ┆ 1 ┆ 1 ┆ 1 │ └─────────────────────┴───────┴───────┴───────┘ If you use an index count in `period` or`offset` , then it’s based on the
values in`index_column` :>>> df = pl.DataFrame({"int": [0, 4, 5, 6, 8], "value": [1, 4, 2, 4, 1]}) >>> df.rolling("int", period="3i").agg(pl.col("int").alias("aggregated")) shape: (5, 2) ┌─────┬────────────┐ │ int ┆ aggregated │ │ --- ┆ --- │ │ i64 ┆ list[i64] │ ╞═════╪════════════╡ │ 0 ┆ [0] │ │ 4 ┆ [4] │ │ 5 ┆ [4, 5] │ │ 6 ┆ [4, 5, 6] │ │ 8 ┆ [6, 8] │ └─────┴────────────┘ If you want the index count to be based on row number, then you may want to combine `rolling` with[`with_row_index()`](api/polars.DataFrame.with_row_index.html#polars.DataFrame.with_row_index) .
 [\[source\]](https://github.com/pola-rs/polars/blob/py-1.43.2/py-polars/src/../src/polars/dataframe/frame.py#L7314-L7470)  - row( )
  - Get the values of a single row, either by index or by predicate. 
    - Parameters:
      - **index**
      - Row index.
      - **by_predicate**
      - Select the row according to a given expression/predicate.
      - **named**
      - Return a dictionary instead of a tuple. The dictionary is a mapping of column name to row value. This is more expensive than returning a regular tuple, but allows for accessing values by column name.
    - Returns:
      - tuple (default) or dictionary of row values
 Warning You should NEVER use this method to iterate over a DataFrame; if you require row-iteration you should strongly prefer use of `iter_rows()` instead.See also Notes The `index` and`by_predicate` params are mutually exclusive. Additionally,
to ensure clarity, the`by_predicate` parameter must be supplied by keyword.When using `by_predicate` it is an error condition if anything other than
one row is returned; more than one row raises`TooManyRowsReturnedError` , and
zero rows will raise`NoRowsReturnedError` (both inherit from`RowsError` ).Examples Specify an index to return the row at the given index as a tuple. >>> df = pl.DataFrame( ... { ... "foo": [1, 2, 3], ... "bar": [6, 7, 8], ... "ham": ["a", "b", "c"], ... } ... ) >>> df.row(2) (3, 8, 'c') Specify `named=True` to get a dictionary instead with a mapping of column
names to row values.>>> df.row(2, named=True) {'foo': 3, 'bar': 8, 'ham': 'c'} Use `by_predicate` to return the row that matches the given predicate.>>> df.row(by_predicate=(pl.col("ham") == "b")) (2, 7, 'b')
 [tuple](https://docs.python.org/3/library/stdtypes.html#tuple) [Any, ...] |[dict](https://docs.python.org/3/library/stdtypes.html#dict) [[str](https://docs.python.org/3/library/stdtypes.html#str) , Any][\[source\]](https://github.com/pola-rs/polars/blob/py-1.43.2/py-polars/src/../src/polars/dataframe/frame.py#L11768-L11881)  - rows(
  - *** ,
  - *named: [bool](https://docs.python.org/3/library/functions.html#bool) = False* ,
  - Returns all data in the DataFrame as a list of rows of python-native values. By default, each row is returned as a tuple of values given in the same order as the frame columns. Setting `named=True` will return rows of dictionaries
instead.
    - Parameters:
      - **named**
      - Return dictionaries instead of tuples. The dictionaries are a mapping of column name to row value. This is more expensive than returning a regular tuple, but allows for accessing values by column name.
    - Returns:
      - list of row value tuples (default), or list of dictionaries (if `named=True` ).
    - list of row value tuples (default), or list of dictionaries (if 
 Warning Row-iteration is not optimal as the underlying data is stored in columnar form; where possible, prefer export via one of the dedicated export/output methods. You should also consider using `iter_rows` instead, to avoid materialising all
the data at once; there is little performance difference between the two, but
peak memory can be reduced if processing rows in batches.See also 
    - [`iter_rows`](api/polars.DataFrame.iter_rows.html#polars.DataFrame.iter_rows)
    - Row iterator over frame data (does not materialise all rows).
    - [`rows_by_key`](api/polars.DataFrame.rows_by_key.html#polars.DataFrame.rows_by_key)
    - Materialises frame data as a key-indexed dictionary.
 Notes If you have `ns` -precision temporal values you should be aware that Python
natively only supports up to`μs` -precision;`ns` -precision values will be
truncated to microseconds on conversion to Python. If this matters to your
use-case you should export to a different format (such as Arrow or NumPy).Examples >>> df = pl.DataFrame( ... { ... "x": ["a", "b", "b", "a"], ... "y": [1, 2, 3, 4], ... "z": [0, 3, 6, 9], ... } ... ) >>> df.rows() [('a', 1, 0), ('b', 2, 3), ('b', 3, 6), ('a', 4, 9)] >>> df.rows(named=True) [{'x': 'a', 'y': 1, 'z': 0}, {'x': 'b', 'y': 2, 'z': 3}, {'x': 'b', 'y': 3, 'z': 6}, {'x': 'a', 'y': 4, 'z': 9}]
 [list](https://docs.python.org/3/library/stdtypes.html#list) [[tuple](https://docs.python.org/3/library/stdtypes.html#tuple) [[Any](https://docs.python.org/3/library/typing.html#typing.Any) , ...]] |[list](https://docs.python.org/3/library/stdtypes.html#list) [[dict](https://docs.python.org/3/library/stdtypes.html#dict) [[str](https://docs.python.org/3/library/stdtypes.html#str) ,[Any](https://docs.python.org/3/library/typing.html#typing.Any) ]][\[source\]](https://github.com/pola-rs/polars/blob/py-1.43.2/py-polars/src/../src/polars/dataframe/frame.py#L11889-L11952)  - rows_by_key(
  - *key: ColumnNameOrSelector | Sequence[ColumnNameOrSelector]* ,
  - *** ,
  - *named: [bool](https://docs.python.org/3/library/functions.html#bool) = False* ,
  - *include_key: [bool](https://docs.python.org/3/library/functions.html#bool) = False* ,
  - *unique: [bool](https://docs.python.org/3/library/functions.html#bool) = False* ,
  - Returns all data as a dictionary of python-native values keyed by some column. This method is like `rows` , but instead of returning rows in a flat list, rows
are grouped by the values in the`key` column(s) and returned as a dictionary.Note that this method should not be used in place of native operations, due to the high cost of materializing all frame data out into a dictionary; it should be used only when you need to move the values out into a Python data structure or other object that cannot operate directly with Polars/Arrow. 
    - Parameters:
      - **key**
      - The column(s) to use as the key for the returned dictionary. If multiple columns are specified, the key will be a tuple of those values, otherwise it will be a string.
      - **named**
      - Return dictionary rows instead of tuples, mapping column name to row value.
      - **include_key**
      - Include key values inline with the associated data (by default the key values are omitted as a memory/performance optimisation, as they can be reoconstructed from the key).
      - **unique**
      - Indicate that the key is unique; this will result in a 1:1 mapping from key to a single associated row. Note that if the key is *not* actually
unique the last row with the given key will be returned.
 See also Notes If you have `ns` -precision temporal values you should be aware that Python
natively only supports up to`μs` -precision;`ns` -precision values will be
truncated to microseconds on conversion to Python. If this matters to your
use-case you should export to a different format (such as Arrow or NumPy).Examples >>> df = pl.DataFrame( ... { ... "w": ["a", "b", "b", "a"], ... "x": ["q", "q", "q", "k"], ... "y": [1.0, 2.5, 3.0, 4.5], ... "z": [9, 8, 7, 6], ... } ... ) Group rows by the given key column(s): >>> df.rows_by_key(key=["w"]) defaultdict(<class 'list'>, {'a': [('q', 1.0, 9), ('k', 4.5, 6)], 'b': [('q', 2.5, 8), ('q', 3.0, 7)]}) Return the same row groupings as dictionaries: >>> df.rows_by_key(key=["w"], named=True) defaultdict(<class 'list'>, {'a': [{'x': 'q', 'y': 1.0, 'z': 9}, {'x': 'k', 'y': 4.5, 'z': 6}], 'b': [{'x': 'q', 'y': 2.5, 'z': 8}, {'x': 'q', 'y': 3.0, 'z': 7}]}) Return row groupings, assuming keys are unique: >>> df.rows_by_key(key=["z"], unique=True) {9: ('a', 'q', 1.0), 8: ('b', 'q', 2.5), 7: ('b', 'q', 3.0), 6: ('a', 'k', 4.5)} Return row groupings as dictionaries, assuming keys are unique: >>> df.rows_by_key(key=["z"], named=True, unique=True) {9: {'w': 'a', 'x': 'q', 'y': 1.0}, 8: {'w': 'b', 'x': 'q', 'y': 2.5}, 7: {'w': 'b', 'x': 'q', 'y': 3.0}, 6: {'w': 'a', 'x': 'k', 'y': 4.5}} Return dictionary rows grouped by a compound key, including key values: >>> df.rows_by_key(key=["w", "x"], named=True, include_key=True) defaultdict(<class 'list'>, {('a', 'q'): [{'w': 'a', 'x': 'q', 'y': 1.0, 'z': 9}], ('b', 'q'): [{'w': 'b', 'x': 'q', 'y': 2.5, 'z': 8}, {'w': 'b', 'x': 'q', 'y': 3.0, 'z': 7}], ('a', 'k'): [{'w': 'a', 'x': 'k', 'y': 4.5, 'z': 6}]})
 [dict](https://docs.python.org/3/library/stdtypes.html#dict) [Any, Any][\[source\]](https://github.com/pola-rs/polars/blob/py-1.43.2/py-polars/src/../src/polars/dataframe/frame.py#L11994-L12120)  - sample(
  - *n: [int](https://docs.python.org/3/library/functions.html#int) | Series | [None](https://docs.python.org/3/library/constants.html#None) = None* ,
  - *** ,
  - *fraction: [float](https://docs.python.org/3/library/functions.html#float) | Series | [None](https://docs.python.org/3/library/constants.html#None) = None* ,
  - *with_replacement: [bool](https://docs.python.org/3/library/functions.html#bool) = False* ,
  - *shuffle: [bool](https://docs.python.org/3/library/functions.html#bool) | [None](https://docs.python.org/3/library/constants.html#None) = None* ,
  - *seed: [int](https://docs.python.org/3/library/functions.html#int) | [None](https://docs.python.org/3/library/constants.html#None) = None* ,
  - Sample from this DataFrame. 
    - Parameters:
      - **n**
      - Number of items to return. Cannot be used with `fraction` . Defaults to 1 if`fraction` is None.
      - **fraction**
      - Fraction of items to return. Cannot be used with `n` .
      - **with_replacement**
      - Allow values to be sampled more than once.
      - **shuffle**
      - Determines the order of the sampled rows. If True, sampled rows are explicitly shuffled. If False, the relative order of the sampled rows is preserved. (i.e. they appear in the same order as the original DataFrame). If None (default), no ordering guarantee; uses the most performant algorithm.
      - **seed**
      - Seed for the random number generator. If set to None (default), a random seed is generated for each time the sample is called.
 Examples >>> df = pl.DataFrame( ... { ... "foo": [1, 2, 3], ... "bar": [6, 7, 8], ... "ham": ["a", "b", "c"], ... } ... ) >>> df.sample(n=2, shuffle=False, seed=0) shape: (2, 3) ┌─────┬─────┬─────┐ │ foo ┆ bar ┆ ham │ │ --- ┆ --- ┆ --- │ │ i64 ┆ i64 ┆ str │ ╞═════╪═════╪═════╡ │ 3 ┆ 8 ┆ c │ │ 2 ┆ 7 ┆ b │ └─────┴─────┴─────┘
 [\[source\]](https://github.com/pola-rs/polars/blob/py-1.43.2/py-polars/src/../src/polars/dataframe/frame.py#L11585-L11655)  - 
*property* schema*: Schema*[\[source\]](https://github.com/pola-rs/polars/blob/py-1.43.2/py-polars/src/../src/polars/dataframe/frame.py#L981-L998)
  - Get an ordered mapping of column names to their data type. Examples >>> df = pl.DataFrame( ... { ... "foo": [1, 2, 3], ... "bar": [6.0, 7.0, 8.0], ... "ham": ["a", "b", "c"], ... } ... ) >>> df.schema Schema({'foo': Int64, 'bar': Float64, 'ham': String})
 
  - select(
  - **exprs: IntoExpr | Iterable[IntoExpr]* ,
  - ***named_exprs: IntoExpr* ,
  - Select columns from this DataFrame. 
    - Parameters:
      - ***exprs**
      - Column(s) to select, specified as positional arguments. Accepts expression input. Strings are parsed as column names, other non-expression inputs are parsed as literals.
      - ****named_exprs**
      - Additional columns to select, specified as keyword arguments. The columns will be renamed to the keyword used.
 Examples Pass the name of a column to select that column. >>> df = pl.DataFrame( ... { ... "foo": [1, 2, 3], ... "bar": [6, 7, 8], ... "ham": ["a", "b", "c"], ... } ... ) >>> df.select("foo") shape: (3, 1) ┌─────┐ │ foo │ │ --- │ │ i64 │ ╞═════╡ │ 1 │ │ 2 │ │ 3 │ └─────┘ Multiple columns can be selected by passing a list of column names. >>> df.select(["foo", "bar"]) shape: (3, 2) ┌─────┬─────┐ │ foo ┆ bar │ │ --- ┆ --- │ │ i64 ┆ i64 │ ╞═════╪═════╡ │ 1 ┆ 6 │ │ 2 ┆ 7 │ │ 3 ┆ 8 │ └─────┴─────┘ Multiple columns can also be selected using positional arguments instead of a list. Expressions are also accepted. >>> df.select(pl.col("foo"), pl.col("bar") + 1) shape: (3, 2) ┌─────┬─────┐ │ foo ┆ bar │ │ --- ┆ --- │ │ i64 ┆ i64 │ ╞═════╪═════╡ │ 1 ┆ 7 │ │ 2 ┆ 8 │ │ 3 ┆ 9 │ └─────┴─────┘ Use keyword arguments to easily name your expression inputs. >>> df.select(threshold=pl.when(pl.col("foo") > 2).then(10).otherwise(0)) shape: (3, 1) ┌───────────┐ │ threshold │ │ --- │ │ i32 │ ╞═══════════╡ │ 0 │ │ 0 │ │ 10 │ └───────────┘
 [\[source\]](https://github.com/pola-rs/polars/blob/py-1.43.2/py-polars/src/../src/polars/dataframe/frame.py#L10449-L10537)  - select_seq(
  - **exprs: IntoExpr | Iterable[IntoExpr]* ,
  - ***named_exprs: IntoExpr* ,
  - Select columns from this DataFrame. This will run all expression sequentially instead of in parallel. Use this when the work per expression is cheap. 
    - Parameters:
      - ***exprs**
      - Column(s) to select, specified as positional arguments. Accepts expression input. Strings are parsed as column names, other non-expression inputs are parsed as literals.
      - ****named_exprs**
      - Additional columns to select, specified as keyword arguments. The columns will be renamed to the keyword used.
 See also
 [\[source\]](https://github.com/pola-rs/polars/blob/py-1.43.2/py-polars/src/../src/polars/dataframe/frame.py#L10539-L10568)  - serialize( )
  - Serialize this DataFrame to a file or string in JSON format. 
    - Parameters:
      - **file**
      - File path or writable file-like object to which the result will be written. If set to `None` (default), the output is returned as a string instead.
      - **format**
      - The format in which to serialize. Options: 
        - `"binary"` : Serialize to binary format (bytes). This is the default.
        - `"json"` : Serialize to JSON format (string).
 Notes Serialization is not stable across Polars versions: a LazyFrame serialized in one Polars version may not be deserializable in another Polars version. Examples Serialize the DataFrame into a binary representation. >>> df = pl.DataFrame( ... { ... "foo": [1, 2, 3], ... "bar": [6, 7, 8], ... } ... ) >>> bytes = df.serialize() >>> type(bytes) <class 'bytes'> The bytes can later be deserialized back into a DataFrame. >>> import io >>> pl.DataFrame.deserialize(io.BytesIO(bytes)) shape: (3, 2) ┌─────┬─────┐ │ foo ┆ bar │ │ --- ┆ --- │ │ i64 ┆ i64 │ ╞═════╪═════╡ │ 1 ┆ 6 │ │ 2 ┆ 7 │ │ 3 ┆ 8 │ └─────┴─────┘
 [bytes](https://docs.python.org/3/library/stdtypes.html#bytes) |[str](https://docs.python.org/3/library/stdtypes.html#str) |[None](https://docs.python.org/3/library/constants.html#None)[\[source\]](https://github.com/pola-rs/polars/blob/py-1.43.2/py-polars/src/../src/polars/dataframe/frame.py#L2797-L2859)  - set_sorted( ) DataFrame
  - Flag a column as sorted. This can speed up future operations. 
    - Parameters:
      - **column**
      - Column that is sorted
      - **descending**
      - Whether the column is sorted in descending order.
      - **nulls_last**
      - Whether the nulls are at the end.
 Warning This can lead to incorrect results if the data is NOT sorted!! Use with care!
 [\[source\]](https://github.com/pola-rs/polars/blob/py-1.43.2/py-polars/src/../src/polars/dataframe/frame.py#L12763-L12798)  - 
*property* shape*: [tuple](https://docs.python.org/3/library/stdtypes.html#tuple)[[int](https://docs.python.org/3/library/functions.html#int), [int](https://docs.python.org/3/library/functions.html#int)]*[\[source\]](https://github.com/pola-rs/polars/blob/py-1.43.2/py-polars/src/../src/polars/dataframe/frame.py#L824-L835)
  - Get the shape of the DataFrame. Examples >>> df = pl.DataFrame({"foo": [1, 2, 3, 4, 5]}) >>> df.shape (5, 1)
 
  - 
shift(*n: [int](https://docs.python.org/3/library/functions.html#int) = 1* ,*** ,*fill_value: IntoExpr | [None](https://docs.python.org/3/library/constants.html#None) = None* )  DataFrame[\[source\]](https://github.com/pola-rs/polars/blob/py-1.43.2/py-polars/src/../src/polars/dataframe/frame.py#L10213-L10290)
  - Shift values by the given number of indices. 
    - Parameters:
      - **n**
      - Number of indices to shift forward. If a negative value is passed, values are shifted in the opposite direction instead.
      - **fill_value**
      - Fill the resulting null values with this value. Accepts scalar expression input. Non-expression inputs are parsed as literals.
 Notes This method is similar to the `LAG` operation in SQL when the value for`n` is positive. With a negative value for`n` , it is similar to`LEAD` .Examples By default, values are shifted forward by one index. >>> df = pl.DataFrame( ... { ... "a": [1, 2, 3, 4], ... "b": [5, 6, 7, 8], ... } ... ) >>> df.shift() shape: (4, 2) ┌──────┬──────┐ │ a ┆ b │ │ --- ┆ --- │ │ i64 ┆ i64 │ ╞══════╪══════╡ │ null ┆ null │ │ 1 ┆ 5 │ │ 2 ┆ 6 │ │ 3 ┆ 7 │ └──────┴──────┘ Pass a negative value to shift in the opposite direction instead. >>> df.shift(-2) shape: (4, 2) ┌──────┬──────┐ │ a ┆ b │ │ --- ┆ --- │ │ i64 ┆ i64 │ ╞══════╪══════╡ │ 3 ┆ 7 │ │ 4 ┆ 8 │ │ null ┆ null │ │ null ┆ null │ └──────┴──────┘ Specify `fill_value` to fill the resulting null values.>>> df.shift(-2, fill_value=100) shape: (4, 2) ┌─────┬─────┐ │ a ┆ b │ │ --- ┆ --- │ │ i64 ┆ i64 │ ╞═════╪═════╡ │ 3 ┆ 7 │ │ 4 ┆ 8 │ │ 100 ┆ 100 │ │ 100 ┆ 100 │ └─────┴─────┘
 
  - show(
  - *limit: [int](https://docs.python.org/3/library/functions.html#int) | [None](https://docs.python.org/3/library/constants.html#None) = 5* ,
  - *** ,
  - *ascii_tables: [bool](https://docs.python.org/3/library/functions.html#bool) | [None](https://docs.python.org/3/library/constants.html#None) = None* ,
  - *decimal_separator: [str](https://docs.python.org/3/library/stdtypes.html#str) | [None](https://docs.python.org/3/library/constants.html#None) = None* ,
  - *thousands_separator: [str](https://docs.python.org/3/library/stdtypes.html#str) | [bool](https://docs.python.org/3/library/functions.html#bool) | [None](https://docs.python.org/3/library/constants.html#None) = None* ,
  - *float_precision: [int](https://docs.python.org/3/library/functions.html#int) | [None](https://docs.python.org/3/library/constants.html#None) = None* ,
  - *fmt_float: FloatFmt | [None](https://docs.python.org/3/library/constants.html#None) = None* ,
  - *fmt_str_lengths: [int](https://docs.python.org/3/library/functions.html#int) | [None](https://docs.python.org/3/library/constants.html#None) = None* ,
  - *fmt_table_cell_list_len: [int](https://docs.python.org/3/library/functions.html#int) | [None](https://docs.python.org/3/library/constants.html#None) = None* ,
  - *tbl_cell_alignment: Alignment | [None](https://docs.python.org/3/library/constants.html#None) = None* ,
  - *tbl_cell_numeric_alignment: Alignment | [None](https://docs.python.org/3/library/constants.html#None) = None* ,
  - *tbl_cols: [int](https://docs.python.org/3/library/functions.html#int) | [None](https://docs.python.org/3/library/constants.html#None) = None* ,
  - *tbl_column_data_type_inline: [bool](https://docs.python.org/3/library/functions.html#bool) | [None](https://docs.python.org/3/library/constants.html#None) = None* ,
  - *tbl_dataframe_shape_below: [bool](https://docs.python.org/3/library/functions.html#bool) | [None](https://docs.python.org/3/library/constants.html#None) = None* ,
  - *tbl_formatting: TableFormatNames | [None](https://docs.python.org/3/library/constants.html#None) = None* ,
  - *tbl_hide_column_data_types: [bool](https://docs.python.org/3/library/functions.html#bool) | [None](https://docs.python.org/3/library/constants.html#None) = None* ,
  - *tbl_hide_column_names: [bool](https://docs.python.org/3/library/functions.html#bool) | [None](https://docs.python.org/3/library/constants.html#None) = None* ,
  - *tbl_hide_dtype_separator: [bool](https://docs.python.org/3/library/functions.html#bool) | [None](https://docs.python.org/3/library/constants.html#None) = None* ,
  - *tbl_hide_dataframe_shape: [bool](https://docs.python.org/3/library/functions.html#bool) | [None](https://docs.python.org/3/library/constants.html#None) = None* ,
  - *tbl_width_chars: [int](https://docs.python.org/3/library/functions.html#int) | [None](https://docs.python.org/3/library/constants.html#None) = None* ,
  - *trim_decimal_zeros: [bool](https://docs.python.org/3/library/functions.html#bool) | [None](https://docs.python.org/3/library/constants.html#None) = True* ,
  - Show the first `n` rows.
    - Parameters:
      - **limit** int
      - Numbers of rows to show. If a negative value is passed, return all rows except the last `abs(n)` . If None is passed, return all rows.
      - **ascii_tables** bool
      - Use ASCII characters to display table outlines. Set False to revert to the default UTF8_FULL_CONDENSED formatting style. See [`Config.set_ascii_tables()`](../api/polars.Config.set_ascii_tables.html#polars.Config.set_ascii_tables) for more information.
      - **decimal_separator** str
      - Set the decimal separator character. See [`Config.set_decimal_separator()`](../api/polars.Config.set_decimal_separator.html#polars.Config.set_decimal_separator) for more information.
      - **thousands_separator** str, bool
      - Set the thousands grouping separator character. See [`Config.set_thousands_separator()`](../api/polars.Config.set_thousands_separator.html#polars.Config.set_thousands_separator) for more information.
      - **float_precision** int
      - Number of decimal places to display for floating point values. See [`Config.set_float_precision()`](../api/polars.Config.set_float_precision.html#polars.Config.set_float_precision) for more information.
      - **fmt_float** {“mixed”, “full”}
      - Control how floating point values are displayed. See [`Config.set_fmt_float()`](../api/polars.Config.set_fmt_float.html#polars.Config.set_fmt_float) for more information. Supported options are:
        - “mixed”: Limit the number of decimal places and use scientific notation for large/small values.
        - “full”: Print the full precision of the floating point number.
      - **fmt_str_lengths** int
      - Number of characters to display for string values. See [`Config.set_fmt_str_lengths()`](../api/polars.Config.set_fmt_str_lengths.html#polars.Config.set_fmt_str_lengths) for more information.
      - **fmt_table_cell_list_len** int
      - Number of elements to display for List values. See [`Config.set_fmt_table_cell_list_len()`](../api/polars.Config.set_fmt_table_cell_list_len.html#polars.Config.set_fmt_table_cell_list_len) for more information.
      - **tbl_cell_alignment** str
      - Set table cell alignment. See [`Config.set_tbl_cell_alignment()`](../api/polars.Config.set_tbl_cell_alignment.html#polars.Config.set_tbl_cell_alignment) for more
information. Supported options are:
        - “LEFT”: left aligned
        - “CENTER”: center aligned
        - “RIGHT”: right aligned
      - **tbl_cell_numeric_alignment** str
      - Set table cell alignment for numeric columns. See [`Config.set_tbl_cell_numeric_alignment()`](../api/polars.Config.set_tbl_cell_numeric_alignment.html#polars.Config.set_tbl_cell_numeric_alignment) for more information.
Supported options are:
        - “LEFT”: left aligned
        - “CENTER”: center aligned
        - “RIGHT”: right aligned
      - **tbl_cols** int
      - Number of columns to display. See [`Config.set_tbl_cols()`](../api/polars.Config.set_tbl_cols.html#polars.Config.set_tbl_cols) for more
information.
      - **tbl_column_data_type_inline** bool
      - Moves the data type inline with the column name (to the right, in parentheses). See [`Config.set_tbl_column_data_type_inline()`](../api/polars.Config.set_tbl_column_data_type_inline.html#polars.Config.set_tbl_column_data_type_inline) for more
information.
      - **tbl_dataframe_shape_below** bool
      - Print the DataFrame shape information below the data when displaying tables. See [`Config.set_tbl_dataframe_shape_below()`](../api/polars.Config.set_tbl_dataframe_shape_below.html#polars.Config.set_tbl_dataframe_shape_below) for more information.
      - **tbl_formatting** str
      - Set table formatting style. See [`Config.set_tbl_formatting()`](../api/polars.Config.set_tbl_formatting.html#polars.Config.set_tbl_formatting) for more
information. Supported options are:
        - “ASCII_FULL”: ASCII, with all borders and lines, including row dividers.
        - “ASCII_FULL_CONDENSED”: Same as ASCII_FULL, but with dense row spacing.
        - “ASCII_NO_BORDERS”: ASCII, no borders.
        - “ASCII_BORDERS_ONLY”: ASCII, borders only.
        - “ASCII_BORDERS_ONLY_CONDENSED”: ASCII, borders only, dense row spacing.
        - “ASCII_HORIZONTAL_ONLY”: ASCII, horizontal lines only.
        - “ASCII_MARKDOWN”: Markdown format (ascii ellipses for truncated values).
        - “MARKDOWN”: Markdown format (utf8 ellipses for truncated values).
        - “UTF8_FULL”: UTF8, with all borders and lines, including row dividers.
        - “UTF8_FULL_CONDENSED”: Same as UTF8_FULL, but with dense row spacing.
        - “UTF8_NO_BORDERS”: UTF8, no borders.
        - “UTF8_BORDERS_ONLY”: UTF8, borders only.
        - “UTF8_HORIZONTAL_ONLY”: UTF8, horizontal lines only.
        - “NOTHING”: No borders or other lines.
      - **tbl_hide_column_data_types** bool
      - Hide table column data types (i64, f64, str etc.). See [`Config.set_tbl_hide_column_data_types()`](../api/polars.Config.set_tbl_hide_column_data_types.html#polars.Config.set_tbl_hide_column_data_types) for more information.
      - **tbl_hide_column_names** bool
      - Hide table column names. See [`Config.set_tbl_hide_column_names()`](../api/polars.Config.set_tbl_hide_column_names.html#polars.Config.set_tbl_hide_column_names) for
more information.
      - **tbl_hide_dtype_separator** bool
      - Hide the ‘—’ separator between the column names and column types. See [`Config.set_tbl_hide_dtype_separator()`](../api/polars.Config.set_tbl_hide_dtype_separator.html#polars.Config.set_tbl_hide_dtype_separator) for more information.
      - **tbl_hide_dataframe_shape** bool
      - Hide the DataFrame shape information when displaying tables. See [`Config.set_tbl_hide_dataframe_shape()`](../api/polars.Config.set_tbl_hide_dataframe_shape.html#polars.Config.set_tbl_hide_dataframe_shape) for more information.
      - **tbl_width_chars** int
      - Set the maximum width of a table in characters. See [`Config.set_tbl_width_chars()`](../api/polars.Config.set_tbl_width_chars.html#polars.Config.set_tbl_width_chars) for more information.
      - **trim_decimal_zeros** bool
      - Strip trailing zeros from Decimal data type values. See [`Config.set_trim_decimal_zeros()`](../api/polars.Config.set_trim_decimal_zeros.html#polars.Config.set_trim_decimal_zeros) for more information.
 See also Examples >>> df = pl.DataFrame( ... { ... "foo": [1, 2, 3, 4, 5], ... "bar": [6, 7, 8, 9, 10], ... "ham": ["a", "b", "c", "d", "e"], ... } ... ) >>> df.show(3) shape: (3, 3) ┌─────┬─────┬─────┐ │ foo ┆ bar ┆ ham │ │ --- ┆ --- ┆ --- │ │ i64 ┆ i64 ┆ str │ ╞═════╪═════╪═════╡ │ 1 ┆ 6 ┆ a │ │ 2 ┆ 7 ┆ b │ │ 3 ┆ 8 ┆ c │ └─────┴─────┴─────┘ Pass a negative value to get all rows `except` the last`abs(n)` .>>> df.show(-3) shape: (2, 3) ┌─────┬─────┬─────┐ │ foo ┆ bar ┆ ham │ │ --- ┆ --- ┆ --- │ │ i64 ┆ i64 ┆ str │ ╞═════╪═════╪═════╡ │ 1 ┆ 6 ┆ a │ │ 2 ┆ 7 ┆ b │ └─────┴─────┴─────┘
 [None](https://docs.python.org/3/library/constants.html#None)[\[source\]](https://github.com/pola-rs/polars/blob/py-1.43.2/py-polars/src/../src/polars/dataframe/frame.py#L13024-L13232)  - 
shrink_to_fit(*** ,*in_place: [bool](https://docs.python.org/3/library/functions.html#bool) = False* )  DataFrame[\[source\]](https://github.com/pola-rs/polars/blob/py-1.43.2/py-polars/src/../src/polars/dataframe/frame.py#L12311-L12323)
  - Shrink DataFrame memory usage. Shrinks to fit the exact capacity needed to hold the data.
 
  - slice( ) DataFrame
  - Get a slice of this DataFrame. 
    - Parameters:
      - **offset**
      - Start index. Negative indexing is supported.
      - **length**
      - Length of the slice. If set to `None` , all rows starting at the offset
will be selected.
 Examples >>> df = pl.DataFrame( ... { ... "foo": [1, 2, 3], ... "bar": [6.0, 7.0, 8.0], ... "ham": ["a", "b", "c"], ... } ... ) >>> df.slice(1, 2) shape: (2, 3) ┌─────┬─────┬─────┐ │ foo ┆ bar ┆ ham │ │ --- ┆ --- ┆ --- │ │ i64 ┆ f64 ┆ str │ ╞═════╪═════╪═════╡ │ 2 ┆ 7.0 ┆ b │ │ 3 ┆ 8.0 ┆ c │ └─────┴─────┴─────┘
 [\[source\]](https://github.com/pola-rs/polars/blob/py-1.43.2/py-polars/src/../src/polars/dataframe/frame.py#L6498-L6532)  - sort(
  - *by: IntoExpr | Iterable[IntoExpr]* ,
  - **more_by: IntoExpr* ,
  - *descending: [bool](https://docs.python.org/3/library/functions.html#bool) | Sequence[[bool](https://docs.python.org/3/library/functions.html#bool)] = False* ,
  - *nulls_last: [bool](https://docs.python.org/3/library/functions.html#bool) | Sequence[[bool](https://docs.python.org/3/library/functions.html#bool)] = False* ,
  - *multithreaded: [bool](https://docs.python.org/3/library/functions.html#bool) = True* ,
  - *maintain_order: [bool](https://docs.python.org/3/library/functions.html#bool) = False* ,
  - Sort the dataframe by the given columns. 
    - Parameters:
      - **by**
      - Column(s) to sort by. Accepts expression input, including selectors. Strings are parsed as column names.
      - ***more_by**
      - Additional columns to sort by, specified as positional arguments.
      - **descending**
      - Sort in descending order. When sorting by multiple columns, can be specified per column by passing a sequence of booleans.
      - **nulls_last**
      - Place null values last; can specify a single boolean applying to all columns or a sequence of booleans for per-column control.
      - **multithreaded**
      - Sort using multiple threads.
      - **maintain_order**
      - Whether the order should be maintained if elements are equal.
 Examples Pass a single column name to sort by that column. >>> df = pl.DataFrame( ... { ... "a": [1, 2, None], ... "b": [6.0, 5.0, 4.0], ... "c": ["a", "c", "b"], ... } ... ) >>> df.sort("a") shape: (3, 3) ┌──────┬─────┬─────┐ │ a ┆ b ┆ c │ │ --- ┆ --- ┆ --- │ │ i64 ┆ f64 ┆ str │ ╞══════╪═════╪═════╡ │ null ┆ 4.0 ┆ b │ │ 1 ┆ 6.0 ┆ a │ │ 2 ┆ 5.0 ┆ c │ └──────┴─────┴─────┘ Sorting by expressions is also supported. >>> df.sort(pl.col("a") + pl.col("b") * 2, nulls_last=True) shape: (3, 3) ┌──────┬─────┬─────┐ │ a ┆ b ┆ c │ │ --- ┆ --- ┆ --- │ │ i64 ┆ f64 ┆ str │ ╞══════╪═════╪═════╡ │ 2 ┆ 5.0 ┆ c │ │ 1 ┆ 6.0 ┆ a │ │ null ┆ 4.0 ┆ b │ └──────┴─────┴─────┘ Sort by multiple columns by passing a list of columns. >>> df.sort(["c", "a"], descending=True) shape: (3, 3) ┌──────┬─────┬─────┐ │ a ┆ b ┆ c │ │ --- ┆ --- ┆ --- │ │ i64 ┆ f64 ┆ str │ ╞══════╪═════╪═════╡ │ 2 ┆ 5.0 ┆ c │ │ null ┆ 4.0 ┆ b │ │ 1 ┆ 6.0 ┆ a │ └──────┴─────┴─────┘ Or use positional arguments to sort by multiple columns in the same way. >>> df.sort("c", "a", descending=[False, True]) shape: (3, 3) ┌──────┬─────┬─────┐ │ a ┆ b ┆ c │ │ --- ┆ --- ┆ --- │ │ i64 ┆ f64 ┆ str │ ╞══════╪═════╪═════╡ │ 1 ┆ 6.0 ┆ a │ │ null ┆ 4.0 ┆ b │ │ 2 ┆ 5.0 ┆ c │ └──────┴─────┴─────┘
 [\[source\]](https://github.com/pola-rs/polars/blob/py-1.43.2/py-polars/src/../src/polars/dataframe/frame.py#L6077-L6187)  - sql( ) DataFrame
  - Execute a SQL query against the DataFrame. Added in version 0.20.24. Warning This functionality is considered **unstable** , although it is close to
being considered stable. It may be changed at any point without it being
considered a breaking change.
    - Parameters:
      - **query**
      - SQL query to execute.
      - **table_name**
      - Optionally provide an explicit name for the table that represents the calling frame (defaults to “self”).
 See also Notes 
    - The calling DataFrame is automatically registered as a table in the SQLContext under the name “self”. If you want access to the DataFrames and LazyFrames found in the current globals, use the top-level [`pl.sql`](../expressions/api/polars.sql.html#polars.sql) .
    - More control over registration and execution behaviour is available by using the [`SQLContext`](../sql/python_api.html#polars.SQLContext) object.
    - The SQL query executes in lazy mode before being collected and returned as a DataFrame.
 Examples >>> from datetime import date >>> df1 = pl.DataFrame( ... { ... "a": [1, 2, 3], ... "b": ["zz", "yy", "xx"], ... "c": [date(1999, 12, 31), date(2010, 10, 10), date(2077, 8, 8)], ... } ... ) Query the DataFrame using SQL: >>> df1.sql("SELECT c, b FROM self WHERE a > 1") shape: (2, 2) ┌────────────┬─────┐ │ c ┆ b │ │ --- ┆ --- │ │ date ┆ str │ ╞════════════╪═════╡ │ 2010-10-10 ┆ yy │ │ 2077-08-08 ┆ xx │ └────────────┴─────┘ Apply transformations to a DataFrame using SQL, aliasing “self” to “frame”. >>> df1.sql( ... query=''' ... SELECT ... a, ... (a % 2 == 0) AS a_is_even, ... CONCAT_WS(':', b, b) AS b_b, ... EXTRACT(year FROM c) AS year, ... 0::float4 AS "zero", ... FROM frame ... ''', ... table_name="frame", ... ) shape: (3, 5) ┌─────┬───────────┬───────┬──────┬──────┐ │ a ┆ a_is_even ┆ b_b ┆ year ┆ zero │ │ --- ┆ --- ┆ --- ┆ --- ┆ --- │ │ i64 ┆ bool ┆ str ┆ i32 ┆ f32 │ ╞═════╪═══════════╪═══════╪══════╪══════╡ │ 1 ┆ false ┆ zz:zz ┆ 1999 ┆ 0.0 │ │ 2 ┆ true ┆ yy:yy ┆ 2010 ┆ 0.0 │ │ 3 ┆ false ┆ xx:xx ┆ 2077 ┆ 0.0 │ └─────┴───────────┴───────┴──────┴──────┘
 [\[source\]](https://github.com/pola-rs/polars/blob/py-1.43.2/py-polars/src/../src/polars/dataframe/frame.py#L6189-L6279)  - 
std(*ddof: [int](https://docs.python.org/3/library/functions.html#int) = 1* )  DataFrame[\[source\]](https://github.com/pola-rs/polars/blob/py-1.43.2/py-polars/src/../src/polars/dataframe/frame.py#L11024-L11065)
  - Aggregate the columns of this DataFrame to their standard deviation value. 
    - Parameters:
      - **ddof**
      - “Delta Degrees of Freedom”: the divisor used in the calculation is N - ddof, where N represents the number of elements. By default ddof is 1.
 Examples >>> df = pl.DataFrame( ... { ... "foo": [1, 2, 3], ... "bar": [6, 7, 8], ... "ham": ["a", "b", "c"], ... } ... ) >>> df.std() shape: (1, 3) ┌─────┬─────┬──────┐ │ foo ┆ bar ┆ ham │ │ --- ┆ --- ┆ --- │ │ f64 ┆ f64 ┆ str │ ╞═════╪═════╪══════╡ │ 1.0 ┆ 1.0 ┆ null │ └─────┴─────┴──────┘ >>> df.std(ddof=0) shape: (1, 3) ┌──────────┬──────────┬──────┐ │ foo ┆ bar ┆ ham │ │ --- ┆ --- ┆ --- │ │ f64 ┆ f64 ┆ str │ ╞══════════╪══════════╪══════╡ │ 0.816497 ┆ 0.816497 ┆ null │ └──────────┴──────────┴──────┘
 
  - 
*property* style*: GT*[\[source\]](https://github.com/pola-rs/polars/blob/py-1.43.2/py-polars/src/../src/polars/dataframe/frame.py#L769-L822)
  - Create a Great Table for styling. Warning This functionality is currently considered **unstable** . It may be
changed at any point without it being considered a breaking change.Polars does not implement styling logic itself, but instead defers to the Great Tables package. Please see the [Great Tables reference](https://posit-dev.github.io/great-tables/reference/) for more information and documentation.Examples Import some styling helpers, and create example data: >>> import polars.selectors as cs >>> from great_tables import loc, style >>> df = pl.DataFrame( ... { ... "site_id": [0, 1, 2], ... "measure_a": [5, 4, 6], ... "measure_b": [7, 3, 3], ... } ... ) Emphasize the site_id as row names: >>> df.style.tab_stub(rowname_col="site_id") Fill the background for the highest measure_a value row: >>> df.style.tab_style( ... style.fill("yellow"), ... loc.body(rows=pl.col("measure_a") == pl.col("measure_a").max()), ... ) Put a spanner (high-level label) over measure columns: >>> df.style.tab_spanner( ... "Measures", cs.starts_with("measure") ... ) Format measure_b values to two decimal places: >>> df.style.fmt_number("measure_b", decimals=2)
 
  - 
sum()  DataFrame[\[source\]](https://github.com/pola-rs/polars/blob/py-1.43.2/py-polars/src/../src/polars/dataframe/frame.py#L10897-L10922)
  - Aggregate the columns of this DataFrame to their sum value. Examples >>> df = pl.DataFrame( ... { ... "foo": [1, 2, 3], ... "bar": [6, 7, 8], ... "ham": ["a", "b", "c"], ... } ... ) >>> df.sum() shape: (1, 3) ┌─────┬─────┬──────┐ │ foo ┆ bar ┆ ham │ │ --- ┆ --- ┆ --- │ │ i64 ┆ i64 ┆ str │ ╞═════╪═════╪══════╡ │ 6 ┆ 21 ┆ null │ └─────┴─────┴──────┘
 
  - 
sum_horizontal(*** ,*ignore_nulls: [bool](https://docs.python.org/3/library/functions.html#bool) = True* )  Series[\[source\]](https://github.com/pola-rs/polars/blob/py-1.43.2/py-polars/src/../src/polars/dataframe/frame.py#L10924-L10958)
  - Sum all values horizontally across columns. 
    - Parameters:
      - **ignore_nulls**
      - Ignore null values (default). If set to `False` , any null value in the input will lead to a null output.
    - Returns:
      - Series
      - A Series named `"sum"` .
 Examples >>> df = pl.DataFrame( ... { ... "foo": [1, 2, 3], ... "bar": [4.0, 5.0, 6.0], ... } ... ) >>> df.sum_horizontal() shape: (3,) Series: 'sum' [f64] [ 5.0 7.0 9.0 ]
 
  - 
tail(*n: [int](https://docs.python.org/3/library/functions.html#int) = 5* )  DataFrame[\[source\]](https://github.com/pola-rs/polars/blob/py-1.43.2/py-polars/src/../src/polars/dataframe/frame.py#L6586-L6636)
  - Get the last `n` rows.
    - Parameters:
      - **n**
      - Number of rows to return. If a negative value is passed, return all rows except the first `abs(n)` .
 Examples >>> df = pl.DataFrame( ... { ... "foo": [1, 2, 3, 4, 5], ... "bar": [6, 7, 8, 9, 10], ... "ham": ["a", "b", "c", "d", "e"], ... } ... ) >>> df.tail(3) shape: (3, 3) ┌─────┬─────┬─────┐ │ foo ┆ bar ┆ ham │ │ --- ┆ --- ┆ --- │ │ i64 ┆ i64 ┆ str │ ╞═════╪═════╪═════╡ │ 3 ┆ 8 ┆ c │ │ 4 ┆ 9 ┆ d │ │ 5 ┆ 10 ┆ e │ └─────┴─────┴─────┘ Pass a negative value to get all rows `except` the first`abs(n)` .>>> df.tail(-3) shape: (2, 3) ┌─────┬─────┬─────┐ │ foo ┆ bar ┆ ham │ │ --- ┆ --- ┆ --- │ │ i64 ┆ i64 ┆ str │ ╞═════╪═════╪═════╡ │ 4 ┆ 9 ┆ d │ │ 5 ┆ 10 ┆ e │ └─────┴─────┴─────┘
 
  - to_arrow(
  - *** ,
  - *compat_level: CompatLevel | [None](https://docs.python.org/3/library/constants.html#None) = None* ,
  - Collect the underlying arrow arrays in an Arrow Table. This operation is mostly zero copy. 
    - Data types that do copy:
      - CategoricalType
 Changed in version 1.1: The `future` parameter was renamed`compat_level` .
    - Parameters:
      - **compat_level**
      - Compatibility level to use when exporting Polars data structures. The default compatibility level is recommended for most users. Use `pl.CompatLevel.oldest()` for the most compatible level.`pl.CompatLevel.newest()` uses the highest supported compatibility
level, but is considered unstable and may change without it being
considered a breaking change.
 Examples >>> df = pl.DataFrame( ... {"foo": [1, 2, 3, 4, 5, 6], "bar": ["a", "b", "c", "d", "e", "f"]} ... ) >>> df.to_arrow() pyarrow.Table foo: int64 bar: large_string ---- foo: [[1,2,3,4,5,6]] bar: [["a","b","c","d","e","f"]]
 [Table](https://arrow.apache.org/docs/python/generated/pyarrow.Table.html#pyarrow.Table)[\[source\]](https://github.com/pola-rs/polars/blob/py-1.43.2/py-polars/src/../src/polars/dataframe/frame.py#L1747-L1805)  - 
to_dict(*** ,*as_series: [bool](https://docs.python.org/3/library/functions.html#bool) = True* )[dict](https://docs.python.org/3/library/stdtypes.html#dict) [[str](https://docs.python.org/3/library/stdtypes.html#str) , Series] |[dict](https://docs.python.org/3/library/stdtypes.html#dict) [[str](https://docs.python.org/3/library/stdtypes.html#str) ,[list](https://docs.python.org/3/library/stdtypes.html#list) [Any]][\[source\]](https://github.com/pola-rs/polars/blob/py-1.43.2/py-polars/src/../src/polars/dataframe/frame.py#L1818-L1911)
  - Convert DataFrame to a dictionary mapping column name to values. 
    - Parameters:
      - **as_series**
      - True -> Values are Series False -> Values are List[Any]
 See also Examples >>> df = pl.DataFrame( ... { ... "A": [1, 2, 3, 4, 5], ... "fruits": ["banana", "banana", "apple", "apple", "banana"], ... "B": [5, 4, 3, 2, 1], ... "cars": ["beetle", "audi", "beetle", "beetle", "beetle"], ... "optional": [28, 300, None, 2, -30], ... } ... ) >>> df shape: (5, 5) ┌─────┬────────┬─────┬────────┬──────────┐ │ A ┆ fruits ┆ B ┆ cars ┆ optional │ │ --- ┆ --- ┆ --- ┆ --- ┆ --- │ │ i64 ┆ str ┆ i64 ┆ str ┆ i64 │ ╞═════╪════════╪═════╪════════╪══════════╡ │ 1 ┆ banana ┆ 5 ┆ beetle ┆ 28 │ │ 2 ┆ banana ┆ 4 ┆ audi ┆ 300 │ │ 3 ┆ apple ┆ 3 ┆ beetle ┆ null │ │ 4 ┆ apple ┆ 2 ┆ beetle ┆ 2 │ │ 5 ┆ banana ┆ 1 ┆ beetle ┆ -30 │ └─────┴────────┴─────┴────────┴──────────┘ >>> df.to_dict(as_series=False) {'A': [1, 2, 3, 4, 5], 'fruits': ['banana', 'banana', 'apple', 'apple', 'banana'], 'B': [5, 4, 3, 2, 1], 'cars': ['beetle', 'audi', 'beetle', 'beetle', 'beetle'], 'optional': [28, 300, None, 2, -30]} >>> df.to_dict(as_series=True) {'A': shape: (5,) Series: 'A' [i64] [ 1 2 3 4 5 ], 'fruits': shape: (5,) Series: 'fruits' [str] [ "banana" "banana" "apple" "apple" "banana" ], 'B': shape: (5,) Series: 'B' [i64] [ 5 4 3 2 1 ], 'cars': shape: (5,) Series: 'cars' [str] [ "beetle" "audi" "beetle" "beetle" "beetle" ], 'optional': shape: (5,) Series: 'optional' [i64] [ 28 300 null 2 -30 ]}
 
  - 
to_dicts()  [list](https://docs.python.org/3/library/stdtypes.html#list) [[dict](https://docs.python.org/3/library/stdtypes.html#dict) [[str](https://docs.python.org/3/library/stdtypes.html#str) ,[Any](https://docs.python.org/3/library/typing.html#typing.Any) ]][\[source\]](https://github.com/pola-rs/polars/blob/py-1.43.2/py-polars/src/../src/polars/dataframe/frame.py#L1913-L1930)
  - Convert every row to a dictionary of Python-native values. Notes If you have `ns` -precision temporal values you should be aware that Python
natively only supports up to`μs` -precision;`ns` -precision values will be
truncated to microseconds on conversion to Python. If this matters to your
use-case you should export to a different format (such as Arrow or NumPy).Examples >>> df = pl.DataFrame({"foo": [1, 2, 3], "bar": [4, 5, 6]}) >>> df.to_dicts() [{'foo': 1, 'bar': 4}, {'foo': 2, 'bar': 5}, {'foo': 3, 'bar': 6}]
 
  - to_dummies(
  - *columns: ColumnNameOrSelector | Sequence[ColumnNameOrSelector] | [None](https://docs.python.org/3/library/constants.html#None) = None* ,
  - *** ,
  - *separator: [str](https://docs.python.org/3/library/stdtypes.html#str) = '_'* ,
  - *drop_first: [bool](https://docs.python.org/3/library/functions.html#bool) = False* ,
  - *drop_nulls: [bool](https://docs.python.org/3/library/functions.html#bool) = False* ,
  - Convert categorical variables into dummy/indicator variables. 
    - Parameters:
      - **columns**
      - Column name(s) or selector(s) that should be converted to dummy variables. If set to `None` (default), convert all columns.
      - **separator**
      - Separator/delimiter used when generating column names.
      - **drop_first**
      - Remove the first category from the variables being encoded.
      - **drop_nulls**
      - If there are `None` values in the series, a`null` column is not generated
 Examples >>> df = pl.DataFrame( ... { ... "foo": [1, 2], ... "bar": [3, 4], ... "ham": ["a", "b"], ... } ... ) >>> df.to_dummies() shape: (2, 6) ┌───────┬───────┬───────┬───────┬───────┬───────┐ │ foo_1 ┆ foo_2 ┆ bar_3 ┆ bar_4 ┆ ham_a ┆ ham_b │ │ --- ┆ --- ┆ --- ┆ --- ┆ --- ┆ --- │ │ u8 ┆ u8 ┆ u8 ┆ u8 ┆ u8 ┆ u8 │ ╞═══════╪═══════╪═══════╪═══════╪═══════╪═══════╡ │ 1 ┆ 0 ┆ 1 ┆ 0 ┆ 1 ┆ 0 │ │ 0 ┆ 1 ┆ 0 ┆ 1 ┆ 0 ┆ 1 │ └───────┴───────┴───────┴───────┴───────┴───────┘ >>> df.to_dummies(drop_first=True) shape: (2, 3) ┌───────┬───────┬───────┐ │ foo_2 ┆ bar_4 ┆ ham_b │ │ --- ┆ --- ┆ --- │ │ u8 ┆ u8 ┆ u8 │ ╞═══════╪═══════╪═══════╡ │ 0 ┆ 0 ┆ 0 │ │ 1 ┆ 1 ┆ 1 │ └───────┴───────┴───────┘ >>> import polars.selectors as cs >>> df.to_dummies(cs.integer(), separator=":") shape: (2, 5) ┌───────┬───────┬───────┬───────┬─────┐ │ foo:1 ┆ foo:2 ┆ bar:3 ┆ bar:4 ┆ ham │ │ --- ┆ --- ┆ --- ┆ --- ┆ --- │ │ u8 ┆ u8 ┆ u8 ┆ u8 ┆ str │ ╞═══════╪═══════╪═══════╪═══════╪═════╡ │ 1 ┆ 0 ┆ 1 ┆ 0 ┆ a │ │ 0 ┆ 1 ┆ 0 ┆ 1 ┆ b │ └───────┴───────┴───────┴───────┴─────┘ >>> df.to_dummies(cs.integer(), drop_first=True, separator=":") shape: (2, 3) ┌───────┬───────┬─────┐ │ foo:2 ┆ bar:4 ┆ ham │ │ --- ┆ --- ┆ --- │ │ u8 ┆ u8 ┆ str │ ╞═══════╪═══════╪═════╡ │ 0 ┆ 0 ┆ a │ │ 1 ┆ 1 ┆ b │ └───────┴───────┴─────┘
 [\[source\]](https://github.com/pola-rs/polars/blob/py-1.43.2/py-polars/src/../src/polars/dataframe/frame.py#L11210-L11291)  - 
to_init_repr(*n: [int](https://docs.python.org/3/library/functions.html#int) = 1000* )[str](https://docs.python.org/3/library/stdtypes.html#str)[\[source\]](https://github.com/pola-rs/polars/blob/py-1.43.2/py-polars/src/../src/polars/dataframe/frame.py#L2727-L2782)
  - Convert DataFrame to instantiable string representation. 
    - Parameters:
      - **n**
      - Only use first n rows.
 Examples >>> df = pl.DataFrame( ... [ ... pl.Series("foo", [1, 2, 3], dtype=pl.UInt8), ... pl.Series("bar", [6.0, 7.0, 8.0], dtype=pl.Float32), ... pl.Series("ham", ["a", "b", "c"], dtype=pl.String), ... ] ... ) >>> print(df.to_init_repr()) pl.DataFrame( [ pl.Series('foo', [1, 2, 3], dtype=pl.UInt8), pl.Series('bar', [6.0, 7.0, 8.0], dtype=pl.Float32), pl.Series('ham', ['a', 'b', 'c'], dtype=pl.String), ] ) >>> df_from_str_repr = eval(df.to_init_repr()) >>> df_from_str_repr shape: (3, 3) ┌─────┬─────┬─────┐ │ foo ┆ bar ┆ ham │ │ --- ┆ --- ┆ --- │ │ u8 ┆ f32 ┆ str │ ╞═════╪═════╪═════╡ │ 1 ┆ 6.0 ┆ a │ │ 2 ┆ 7.0 ┆ b │ │ 3 ┆ 8.0 ┆ c │ └─────┴─────┴─────┘
 
  - to_jax(
  - *return_type: JaxExportType = 'array'* ,
  - *** ,
  - *device: jax.Device | [str](https://docs.python.org/3/library/stdtypes.html#str) | [None](https://docs.python.org/3/library/constants.html#None) = None* ,
  - *label: [str](https://docs.python.org/3/library/stdtypes.html#str) | Expr | Sequence[[str](https://docs.python.org/3/library/stdtypes.html#str) | Expr] | [None](https://docs.python.org/3/library/constants.html#None) = None* ,
  - *features: [str](https://docs.python.org/3/library/stdtypes.html#str) | Expr | Sequence[[str](https://docs.python.org/3/library/stdtypes.html#str) | Expr] | [None](https://docs.python.org/3/library/constants.html#None) = None* ,
  - *dtype: PolarsDataType | [None](https://docs.python.org/3/library/constants.html#None) = None* ,
  - *order: IndexOrder = 'fortran'* ,
  - Convert DataFrame to a Jax Array, or dict of Jax Arrays. Added in version 0.20.27. Warning This functionality is currently considered **unstable** . It may be
changed at any point without it being considered a breaking change.
    - Parameters:
      - **return_type** {“array”, “dict”}
      - Set return type; a Jax Array, or dict of Jax Arrays.
      - **device**
      - Specify the jax `Device` on which the array will be created; can provide
a string (such as “cpu”, “gpu”, or “tpu”) in which case the device is
retrieved as`jax.devices(string)[0]` . For more specific control you
can supply the instantiated`Device` directly. If None, arrays are
created on the default device.
      - **label**
      - One or more column names, expressions, or selectors that label the feature data; results in a `{"label": ..., "features": ...}` dict being returned
when`return_type` is “dict” instead of a`{"col": array, }` dict.
      - **features**
      - One or more column names, expressions, or selectors that contain the feature data; if omitted, all columns that are not designated as part of the label are used. Only applies when `return_type` is “dict”.
      - **dtype**
      - Unify the dtype of all returned arrays; this casts any column that is not already of the required dtype before converting to Array. Note that export will be single-precision (32bit) unless the Jax config/environment directs otherwise (eg: “jax_enable_x64” was set True in the config object at startup, or “JAX_ENABLE_X64” is set to “1” in the environment).
      - **order** {“c”, “fortran”}
      - The index order of the returned Jax array, either C-like (row-major) or Fortran-like (column-major).
 See also Examples >>> df = pl.DataFrame( ... { ... "lbl": [0, 1, 2, 3], ... "feat1": [1, 0, 0, 1], ... "feat2": [1.5, -0.5, 0.0, -2.25], ... } ... ) Standard return type (2D Array), on the standard device: >>> df.to_jax() Array([[ 0. , 1. , 1.5 ], [ 1. , 0. , -0.5 ], [ 2. , 0. , 0. ], [ 3. , 1. , -2.25]], dtype=float32) Create the Array on the default GPU device: >>> a = df.to_jax(device="gpu") >>> a.device() GpuDevice(id=0, process_index=0) Create the Array on a specific GPU device: >>> gpu_device = jax.devices("gpu")[1] >>> a = df.to_jax(device=gpu_device) >>> a.device() GpuDevice(id=1, process_index=0) As a dictionary of individual Arrays: >>> df.to_jax("dict") {'lbl': Array([0, 1, 2, 3], dtype=int32), 'feat1': Array([1, 0, 0, 1], dtype=int32), 'feat2': Array([ 1.5 , -0.5 , 0. , -2.25], dtype=float32)} As a “label” and “features” dictionary; note that as “features” is not declared, it defaults to all the columns that are not in “label”: >>> df.to_jax("dict", label="lbl") {'label': Array([[0], [1], [2], [3]], dtype=int32), 'features': Array([[ 1. , 1.5 ], [ 0. , -0.5 ], [ 0. , 0. ], [ 1. , -2.25]], dtype=float32)} As a “label” and “features” dictionary where each is designated using a col or selector expression (which can also be used to cast the data if the label and features are better-represented with different dtypes): >>> import polars.selectors as cs >>> df.to_jax( ... return_type="dict", ... features=cs.float(), ... label=pl.col("lbl").cast(pl.UInt8), ... ) {'label': Array([[0], [1], [2], [3]], dtype=uint8), 'features': Array([[ 1.5 ], [-0.5 ], [ 0. ], [-2.25]], dtype=float32)}
 [dict](https://docs.python.org/3/library/stdtypes.html#dict) [[str](https://docs.python.org/3/library/stdtypes.html#str) , jax.Array][\[source\]](https://github.com/pola-rs/polars/blob/py-1.43.2/py-polars/src/../src/polars/dataframe/frame.py#L2109-L2291)  - to_numpy(
  - *** ,
  - *order: IndexOrder = 'fortran'* ,
  - *writable: [bool](https://docs.python.org/3/library/functions.html#bool) = False* ,
  - *allow_copy: [bool](https://docs.python.org/3/library/functions.html#bool) = True* ,
  - *structured: [bool](https://docs.python.org/3/library/functions.html#bool) = False* ,
  - *use_pyarrow: [bool](https://docs.python.org/3/library/functions.html#bool) | [None](https://docs.python.org/3/library/constants.html#None) = None* ,
  - Convert this DataFrame to a NumPy ndarray. This operation copies data only when necessary. The conversion is zero copy when all of the following hold: 
    - The DataFrame is fully contiguous in memory, with all Series back-to-back and all Series consisting of a single chunk.
    - The data type is an integer or float.
    - The DataFrame contains no null values.
    - The `order` parameter is set to`fortran` (default).
    - The `writable` parameter is set to`False` (default).
 
    - Parameters:
      - **order**
      - The index order of the returned NumPy array, either C-like or Fortran-like. In general, using the Fortran-like index order is faster. However, the C-like order might be more appropriate to use for downstream applications to prevent cloning data, e.g. when reshaping into a one-dimensional array.
      - **writable**
      - Ensure the resulting array is writable. This will force a copy of the data if the array was created without copy, as the underlying Arrow data is immutable.
      - **allow_copy**
      - Allow memory to be copied to perform the conversion. If set to `False` ,
causes conversions that are not zero-copy to fail.
      - **structured**
      - Return a [structured array](https://numpy.org/doc/stable/user/basics.rec.html) with a data type that corresponds to the
DataFrame schema. If set to`False` (default), a 2D ndarray is
returned instead.
      - **use_pyarrow**
      - Use [pyarrow.Array.to_numpy](https://arrow.apache.org/docs/python/generated/pyarrow.Array.html#pyarrow.Array.to_numpy)function for the conversion to NumPy if necessary. Deprecated since version 0.20.28: Polars now uses its native engine by default for conversion to NumPy.
 Examples Numeric data without nulls can be converted without copying data in some cases. The resulting array will not be writable. >>> df = pl.DataFrame({"a": [1, 2, 3]}) >>> arr = df.to_numpy() >>> arr array([[1], [2], [3]]) >>> arr.flags.writeable False Set `writable=True` to force data copy to make the array writable.>>> df.to_numpy(writable=True).flags.writeable True If the DataFrame contains different numeric data types, the resulting data type will be the supertype. This requires data to be copied. Integer types with nulls are cast to a float type with `nan` representing a null value.>>> df = pl.DataFrame({"a": [1, 2, None], "b": [4.0, 5.0, 6.0]}) >>> df.to_numpy() array([[ 1., 4.], [ 2., 5.], [nan, 6.]]) Set `allow_copy=False` to raise an error if data would be copied.>>> s.to_numpy(allow_copy=False) Traceback (most recent call last): ... RuntimeError: copy not allowed: cannot convert to a NumPy array without copying data Polars defaults to F-contiguous order. Use `order="c"` to force the resulting
array to be C-contiguous.>>> df.to_numpy(order="c").flags.c_contiguous True DataFrames with mixed types will result in an array with an object dtype. >>> df = pl.DataFrame( ... { ... "foo": [1, 2, 3], ... "bar": [6.5, 7.0, 8.5], ... "ham": ["a", "b", "c"], ... }, ... schema_overrides={"foo": pl.UInt8, "bar": pl.Float32}, ... ) >>> df.to_numpy() array([[1, 6.5, 'a'], [2, 7.0, 'b'], [3, 8.5, 'c']], dtype=object) Set `structured=True` to convert to a structured array, which can better
preserve individual column data such as name and data type.>>> df.to_numpy(structured=True) array([(1, 6.5, 'a'), (2, 7. , 'b'), (3, 8.5, 'c')], dtype=[('foo', 'u1'), ('bar', '<f4'), ('ham', '<U1')])
 [\[source\]](https://github.com/pola-rs/polars/blob/py-1.43.2/py-polars/src/../src/polars/dataframe/frame.py#L1932-L2083)  - to_pandas( )
  - Convert this DataFrame to a pandas DataFrame. This operation copies data if `use_pyarrow_extension_array` is not enabled.
    - Parameters:
      - **use_pyarrow_extension_array**
      - Use PyArrow-backed extension arrays instead of NumPy arrays for the columns of the pandas DataFrame. This allows zero copy operations and preservation of null values. Subsequent operations on the resulting pandas DataFrame may trigger conversion to NumPy if those operations are not supported by PyArrow compute functions.
      - ****kwargs**
      - Additional keyword arguments to be passed to [`pyarrow.Table.to_pandas()`](https://arrow.apache.org/docs/python/generated/pyarrow.Table.html#pyarrow.Table.to_pandas) .
    - Returns:
 Notes This operation requires that both [`pandas`](https://pandas.pydata.org/docs/index.html#module-pandas) and`pyarrow` are
installed.Examples >>> df = pl.DataFrame( ... { ... "foo": [1, 2, 3], ... "bar": [6.0, 7.0, 8.0], ... "ham": ["a", "b", "c"], ... } ... ) >>> df.to_pandas() foo bar ham 0 1 6.0 a 1 2 7.0 b 2 3 8.0 c Null values in numeric columns are converted to `NaN` .>>> df = pl.DataFrame( ... { ... "foo": [1, 2, None], ... "bar": [6.0, None, 8.0], ... "ham": [None, "b", "c"], ... } ... ) >>> df.to_pandas() foo bar ham 0 1.0 6.0 NaN 1 2.0 NaN b 2 NaN 8.0 c Pass `use_pyarrow_extension_array=True` to get a pandas DataFrame with columns
backed by PyArrow extension arrays. This will preserve null values.>>> df.to_pandas(use_pyarrow_extension_array=True) foo bar ham 0 1 6.0 <NA> 1 2 <NA> b 2 <NA> 8.0 c >>> _.dtypes foo int64[pyarrow] bar double[pyarrow] ham large_string[pyarrow] dtype: object
 [DataFrame](https://pandas.pydata.org/docs/reference/api/pandas.DataFrame.html#pandas.DataFrame)[\[source\]](https://github.com/pola-rs/polars/blob/py-1.43.2/py-polars/src/../src/polars/dataframe/frame.py#L2533-L2632)  - 
to_series(*index: [int](https://docs.python.org/3/library/functions.html#int) = 0* )  Series[\[source\]](https://github.com/pola-rs/polars/blob/py-1.43.2/py-polars/src/../src/polars/dataframe/frame.py#L2694-L2725)
  - Select column as Series at index location. 
    - Parameters:
      - **index**
      - Location of selection.
 See also Examples >>> df = pl.DataFrame( ... { ... "foo": [1, 2, 3], ... "bar": [6, 7, 8], ... "ham": ["a", "b", "c"], ... } ... ) >>> df.to_series(1) shape: (3,) Series: 'bar' [i64] [ 6 7 8 ]
 
  - 
to_struct(*name: [str](https://docs.python.org/3/library/stdtypes.html#str) = ''* )  Series[\[source\]](https://github.com/pola-rs/polars/blob/py-1.43.2/py-polars/src/../src/polars/dataframe/frame.py#L12460-L12488)
  - Convert a `DataFrame` to a`Series` of type`Struct` .
    - Parameters:
      - **name**
      - Name for the struct Series
 Examples >>> df = pl.DataFrame( ... { ... "a": [1, 2, 3, 4, 5], ... "b": ["one", "two", "three", "four", "five"], ... } ... ) >>> df.to_struct("nums") shape: (5,) Series: 'nums' [struct[2]] [ {1,"one"} {2,"two"} {3,"three"} {4,"four"} {5,"five"} ]
 
  - to_torch(
  - *return_type: TorchExportType = 'tensor'* ,
  - *** ,
  - *label: [str](https://docs.python.org/3/library/stdtypes.html#str) | Expr | Sequence[[str](https://docs.python.org/3/library/stdtypes.html#str) | Expr] | [None](https://docs.python.org/3/library/constants.html#None) = None* ,
  - *features: [str](https://docs.python.org/3/library/stdtypes.html#str) | Expr | Sequence[[str](https://docs.python.org/3/library/stdtypes.html#str) | Expr] | [None](https://docs.python.org/3/library/constants.html#None) = None* ,
  - *dtype: PolarsDataType | [None](https://docs.python.org/3/library/constants.html#None) = None* ,
  - Convert DataFrame to a PyTorch Tensor, Dataset, or dict of Tensors. Added in version 0.20.23. Warning This functionality is currently considered **unstable** . It may be
changed at any point without it being considered a breaking change.
    - Parameters:
      - **return_type** {“tensor”, “dataset”, “dict”}
      - Set return type; a PyTorch Tensor, PolarsDataset (a frame-specialized TensorDataset), or dict of Tensors.
      - **label**
      - One or more column names, expressions, or selectors that label the feature data; when `return_type` is “dataset”, the PolarsDataset will return`(features, label)` tensor tuples for each row. Otherwise, it returns`(features,)` tensor tuples where the feature contains all the row data.
      - **features**
      - One or more column names, expressions, or selectors that contain the feature data; if omitted, all columns that are not designated as part of the label are used.
      - **dtype**
      - Unify the dtype of all returned tensors; this casts any column that is not of the required dtype before converting to Tensor. This includes the label column *unless* the label is an expression (such as`pl.col("label_column").cast(pl.Int16)` ).
 See also Examples >>> df = pl.DataFrame( ... { ... "lbl": [0, 1, 2, 3], ... "feat1": [1, 0, 0, 1], ... "feat2": [1.5, -0.5, 0.0, -2.25], ... } ... ) Standard return type (Tensor), with f32 supertype: >>> df.to_torch(dtype=pl.Float32) tensor([[ 0.0000, 1.0000, 1.5000], [ 1.0000, 0.0000, -0.5000], [ 2.0000, 0.0000, 0.0000], [ 3.0000, 1.0000, -2.2500]]) As a dictionary of individual Tensors: >>> df.to_torch("dict") {'lbl': tensor([0, 1, 2, 3]), 'feat1': tensor([1, 0, 0, 1]), 'feat2': tensor([ 1.5000, -0.5000, 0.0000, -2.2500], dtype=torch.float64)} As a “label” and “features” dictionary; note that as “features” is not declared, it defaults to all the columns that are not in “label”: >>> df.to_torch("dict", label="lbl", dtype=pl.Float32) {'label': tensor([[0.], [1.], [2.], [3.]]), 'features': tensor([[ 1.0000, 1.5000], [ 0.0000, -0.5000], [ 0.0000, 0.0000], [ 1.0000, -2.2500]])} As a PolarsDataset, with f64 supertype: >>> ds = df.to_torch("dataset", dtype=pl.Float64) >>> ds[3] (tensor([ 3.0000, 1.0000, -2.2500], dtype=torch.float64),) >>> ds[:2] (tensor([[ 0.0000, 1.0000, 1.5000], [ 1.0000, 0.0000, -0.5000]], dtype=torch.float64),) >>> ds[[0, 3]] (tensor([[ 0.0000, 1.0000, 1.5000], [ 3.0000, 1.0000, -2.2500]], dtype=torch.float64),) As a convenience the PolarsDataset can opt in to half-precision data for experimentation (usually this would be set on the model/pipeline): >>> list(ds.half()) [(tensor([0.0000, 1.0000, 1.5000], dtype=torch.float16),), (tensor([ 1.0000, 0.0000, -0.5000], dtype=torch.float16),), (tensor([2., 0., 0.], dtype=torch.float16),), (tensor([ 3.0000, 1.0000, -2.2500], dtype=torch.float16),)] Pass PolarsDataset to a DataLoader, designating the label: >>> from torch.utils.data import DataLoader >>> ds = df.to_torch("dataset", label="lbl") >>> dl = DataLoader(ds, batch_size=2) >>> batches = list(dl) >>> batches[0] [tensor([[ 1.0000, 1.5000], [ 0.0000, -0.5000]], dtype=torch.float64), tensor([0, 1])] Note that labels can be given as expressions, allowing them to have a dtype independent of the feature columns (multi-column labels are supported). >>> ds = df.to_torch( ... return_type="dataset", ... dtype=pl.Float32, ... label=pl.col("lbl").cast(pl.Int16), ... ) >>> ds[:2] (tensor([[ 1.0000, 1.5000], [ 0.0000, -0.5000]]), tensor([0, 1], dtype=torch.int16)) Easily integrate with (for example) scikit-learn and other datasets: >>> from sklearn.datasets import fetch_california_housing >>> housing = fetch_california_housing() >>> df = pl.DataFrame( ... data=housing.data, ... schema=housing.feature_names, ... ).with_columns( ... Target=housing.target, ... ) >>> train = df.to_torch("dataset", label="Target") >>> loader = DataLoader( ... train, ... shuffle=True, ... batch_size=64, ... )
 [dict](https://docs.python.org/3/library/stdtypes.html#dict) [[str](https://docs.python.org/3/library/stdtypes.html#str) , torch.Tensor] | PolarsDataset[\[source\]](https://github.com/pola-rs/polars/blob/py-1.43.2/py-polars/src/../src/polars/dataframe/frame.py#L2323-L2531)  - top_k( ) DataFrame
  - Return the `k` largest rows.Non-null elements are always preferred over null elements, regardless of the value of `reverse` . The output is not guaranteed to be in any
particular order, call[`sort()`](api/polars.DataFrame.sort.html#polars.DataFrame.sort) after this function if you wish the
output to be sorted.Changed in version 1.0.0: The `descending` parameter was renamed`reverse` .
    - Parameters:
      - **k**
      - Number of rows to return.
      - **by**
      - Column(s) used to determine the top rows. Accepts expression input. Strings are parsed as column names.
      - **reverse**
      - Consider the `k` smallest elements of the`by` column(s) (instead of the`k` largest). This can be specified per column by passing a sequence of
booleans.
 See also Examples >>> df = pl.DataFrame( ... { ... "a": ["a", "b", "a", "b", "b", "c"], ... "b": [2, 1, 1, 3, 2, 1], ... } ... ) Get the rows which contain the 4 largest values in column b. >>> df.top_k(4, by="b") shape: (4, 2) ┌─────┬─────┐ │ a ┆ b │ │ --- ┆ --- │ │ str ┆ i64 │ ╞═════╪═════╡ │ b ┆ 3 │ │ a ┆ 2 │ │ b ┆ 2 │ │ b ┆ 1 │ └─────┴─────┘ Get the rows which contain the 4 largest values when sorting on column b and a. >>> df.top_k(4, by=["b", "a"]) shape: (4, 2) ┌─────┬─────┐ │ a ┆ b │ │ --- ┆ --- │ │ str ┆ i64 │ ╞═════╪═════╡ │ b ┆ 3 │ │ b ┆ 2 │ │ a ┆ 2 │ │ c ┆ 1 │ └─────┴─────┘
 [\[source\]](https://github.com/pola-rs/polars/blob/py-1.43.2/py-polars/src/../src/polars/dataframe/frame.py#L6281-L6368)  - transpose(
  - *** ,
  - *include_header: [bool](https://docs.python.org/3/library/functions.html#bool) = False* ,
  - *header_name: [str](https://docs.python.org/3/library/stdtypes.html#str) = 'column'* ,
  - *column_names: [str](https://docs.python.org/3/library/stdtypes.html#str) | [Iterable](https://docs.python.org/3/library/collections.abc.html#collections.abc.Iterable)[[str](https://docs.python.org/3/library/stdtypes.html#str)] | [None](https://docs.python.org/3/library/constants.html#None) = None* ,
  - Transpose a DataFrame over the diagonal. 
    - Parameters:
      - **include_header**
      - If set, the column names will be added as first column.
      - **header_name**
      - If `include_header` is set, this determines the name of the column that will
be inserted.
      - **column_names**
      - Optional iterable yielding strings or a string naming an existing column. These will name the value (non-header) columns in the transposed data.
    - Returns:
      - DataFrame
 Notes This is a very expensive operation. Perhaps you can do it differently. Examples >>> df = pl.DataFrame({"a": [1, 2, 3], "b": [4, 5, 6]}) >>> df.transpose(include_header=True) shape: (2, 4) ┌────────┬──────────┬──────────┬──────────┐ │ column ┆ column_0 ┆ column_1 ┆ column_2 │ │ --- ┆ --- ┆ --- ┆ --- │ │ str ┆ i64 ┆ i64 ┆ i64 │ ╞════════╪══════════╪══════════╪══════════╡ │ a ┆ 1 ┆ 2 ┆ 3 │ │ b ┆ 4 ┆ 5 ┆ 6 │ └────────┴──────────┴──────────┴──────────┘ Replace the auto-generated column names with a list >>> df.transpose(include_header=False, column_names=["x", "y", "z"]) shape: (2, 3) ┌─────┬─────┬─────┐ │ x ┆ y ┆ z │ │ --- ┆ --- ┆ --- │ │ i64 ┆ i64 ┆ i64 │ ╞═════╪═════╪═════╡ │ 1 ┆ 2 ┆ 3 │ │ 4 ┆ 5 ┆ 6 │ └─────┴─────┴─────┘ Include the header as a separate column >>> df.transpose( ... include_header=True, header_name="foo", column_names=["x", "y", "z"] ... ) shape: (2, 4) ┌─────┬─────┬─────┬─────┐ │ foo ┆ x ┆ y ┆ z │ │ --- ┆ --- ┆ --- ┆ --- │ │ str ┆ i64 ┆ i64 ┆ i64 │ ╞═════╪═════╪═════╪═════╡ │ a ┆ 1 ┆ 2 ┆ 3 │ │ b ┆ 4 ┆ 5 ┆ 6 │ └─────┴─────┴─────┴─────┘ Replace the auto-generated column with column names from a generator function >>> def name_generator(): ... base_name = "my_column_" ... count = 0 ... while True: ... yield f"{base_name}{count}" ... count += 1 >>> df.transpose(include_header=False, column_names=name_generator()) shape: (2, 3) ┌─────────────┬─────────────┬─────────────┐ │ my_column_0 ┆ my_column_1 ┆ my_column_2 │ │ --- ┆ --- ┆ --- │ │ i64 ┆ i64 ┆ i64 │ ╞═════════════╪═════════════╪═════════════╡ │ 1 ┆ 2 ┆ 3 │ │ 4 ┆ 5 ┆ 6 │ └─────────────┴─────────────┴─────────────┘ Use an existing column as the new column names >>> df = pl.DataFrame(dict(id=["i", "j", "k"], a=[1, 2, 3], b=[4, 5, 6])) >>> df.transpose(column_names="id") shape: (2, 3) ┌─────┬─────┬─────┐ │ i ┆ j ┆ k │ │ --- ┆ --- ┆ --- │ │ i64 ┆ i64 ┆ i64 │ ╞═════╪═════╪═════╡ │ 1 ┆ 2 ┆ 3 │ │ 4 ┆ 5 ┆ 6 │ └─────┴─────┴─────┘ >>> df.transpose(include_header=True, header_name="new_id", column_names="id") shape: (2, 4) ┌────────┬─────┬─────┬─────┐ │ new_id ┆ i ┆ j ┆ k │ │ --- ┆ --- ┆ --- ┆ --- │ │ str ┆ i64 ┆ i64 ┆ i64 │ ╞════════╪═════╪═════╪═════╡ │ a ┆ 1 ┆ 2 ┆ 3 │ │ b ┆ 4 ┆ 5 ┆ 6 │ └────────┴─────┴─────┴─────┘
 [\[source\]](https://github.com/pola-rs/polars/blob/py-1.43.2/py-polars/src/../src/polars/dataframe/frame.py#L5115-L5235)  - unique(
  - *subset: IntoExpr | Collection[IntoExpr] | [None](https://docs.python.org/3/library/constants.html#None) = None* ,
  - *** ,
  - *keep: UniqueKeepStrategy = 'any'* ,
  - *maintain_order: [bool](https://docs.python.org/3/library/functions.html#bool) = False* ,
  - Drop duplicate rows from this DataFrame. 
    - Parameters:
      - **subset**
      - Column name(s), selector(s), or expressions to consider when identifying duplicate rows. If set to `None` (default), all columns are considered.
      - **keep** {‘first’, ‘last’, ‘any’, ‘none’}
      - Which of the duplicate rows to keep. 
          - ‘any’: Does not give any guarantee of which row is kept.
          - This allows more optimizations.
        - ‘none’: Don’t keep duplicate rows.
        - ‘first’: Keep the first unique row.
        - ‘last’: Keep the last unique row.
      - **maintain_order**
      - Keep the same order as the original DataFrame. This is more expensive to compute. Settings this to `True` blocks the possibility to run on
the streaming engine.
    - Returns:
      - DataFrame
      - DataFrame with unique rows.
 Notes If you’re coming from Pandas, this is similar to `pandas.DataFrame.drop_duplicates` .Examples >>> df = pl.DataFrame( ... { ... "foo": [1, 2, 3, 1, 1], ... "bar": ["a", "a", "a", "x", "x"], ... "ham": ["b", "b", "b", "y", "y"], ... } ... ) By default, all columns are considered when determining which rows are unique: >>> df.unique(maintain_order=True) shape: (4, 3) ┌─────┬─────┬─────┐ │ foo ┆ bar ┆ ham │ │ --- ┆ --- ┆ --- │ │ i64 ┆ str ┆ str │ ╞═════╪═════╪═════╡ │ 1 ┆ a ┆ b │ │ 2 ┆ a ┆ b │ │ 3 ┆ a ┆ b │ │ 1 ┆ x ┆ y │ └─────┴─────┴─────┘ We can also consider only a subset of columns when determining uniqueness, controlling which row we keep when duplicates are found: >>> df.unique(subset="foo", keep="first", maintain_order=True) shape: (3, 3) ┌─────┬─────┬─────┐ │ foo ┆ bar ┆ ham │ │ --- ┆ --- ┆ --- │ │ i64 ┆ str ┆ str │ ╞═════╪═════╪═════╡ │ 1 ┆ a ┆ b │ │ 2 ┆ a ┆ b │ │ 3 ┆ a ┆ b │ └─────┴─────┴─────┘ >>> df.unique(subset="foo", keep="last", maintain_order=True) shape: (3, 3) ┌─────┬─────┬─────┐ │ foo ┆ bar ┆ ham │ │ --- ┆ --- ┆ --- │ │ i64 ┆ str ┆ str │ ╞═════╪═════╪═════╡ │ 2 ┆ a ┆ b │ │ 3 ┆ a ┆ b │ │ 1 ┆ x ┆ y │ └─────┴─────┴─────┘ >>> df.unique(subset="foo", keep="none", maintain_order=True) shape: (2, 3) ┌─────┬─────┬─────┐ │ foo ┆ bar ┆ ham │ │ --- ┆ --- ┆ --- │ │ i64 ┆ str ┆ str │ ╞═════╪═════╪═════╡ │ 2 ┆ a ┆ b │ │ 3 ┆ a ┆ b │ └─────┴─────┴─────┘ Selectors can be used to define the “subset” parameter: >>> import polars.selectors as cs >>> df.unique(subset=cs.string(), maintain_order=True) shape: (2, 3) ┌─────┬─────┬─────┐ │ foo ┆ bar ┆ ham │ │ --- ┆ --- ┆ --- │ │ i64 ┆ str ┆ str │ ╞═════╪═════╪═════╡ │ 1 ┆ a ┆ b │ │ 1 ┆ x ┆ y │ └─────┴─────┴─────┘ We can also use an arbitrary expression in the “subset” parameter; in this example we use the part of the label in front of “:” to determine uniqueness: >>> df = pl.DataFrame( ... { ... "label": ["xx:1", "xx:2", "yy:3", "yy:4"], ... "value": [100, 200, 300, 400], ... } ... ) >>> df.unique( ... subset=pl.col("label").str.extract(r"^(\w+):"), ... maintain_order=True, ... keep="first", ... ) shape: (2, 2) ┌───────┬───────┐ │ label ┆ value │ │ --- ┆ --- │ │ str ┆ i64 │ ╞═══════╪═══════╡ │ xx:1 ┆ 100 │ │ yy:3 ┆ 300 │ └───────┴───────┘
 [\[source\]](https://github.com/pola-rs/polars/blob/py-1.43.2/py-polars/src/../src/polars/dataframe/frame.py#L11293-L11436)  - unnest(
  - *columns: ColumnNameOrSelector | Collection[ColumnNameOrSelector] | [None](https://docs.python.org/3/library/constants.html#None) = None* ,
  - **more_columns: ColumnNameOrSelector* ,
  - *separator: [str](https://docs.python.org/3/library/stdtypes.html#str) | [None](https://docs.python.org/3/library/constants.html#None) = None* ,
  - Decompose struct columns into separate columns for each of their fields. The new columns will be inserted into the dataframe at the location of the struct column. If no columns are provided, all struct columns are unnested. 
    - Parameters:
      - **columns**
      - Name of the struct column(s) that should be unnested.
      - ***more_columns**
      - Additional columns to unnest, specified as positional arguments.
      - **separator**
      - Rename output column names as combination of the struct column name, name separator and field name.
 Examples >>> df = pl.DataFrame( ... { ... "before": ["foo", "bar"], ... "t_a": [1, 2], ... "t_b": ["a", "b"], ... "t_c": [True, None], ... "t_d": [[1, 2], [3]], ... "after": ["baz", "womp"], ... } ... ).select("before", pl.struct(pl.col("^t_.$")).alias("t_struct"), "after") >>> df shape: (2, 3) ┌────────┬─────────────────────┬───────┐ │ before ┆ t_struct ┆ after │ │ --- ┆ --- ┆ --- │ │ str ┆ struct[4] ┆ str │ ╞════════╪═════════════════════╪═══════╡ │ foo ┆ {1,"a",true,[1, 2]} ┆ baz │ │ bar ┆ {2,"b",null,[3]} ┆ womp │ └────────┴─────────────────────┴───────┘ >>> df.unnest("t_struct") shape: (2, 6) ┌────────┬─────┬─────┬──────┬───────────┬───────┐ │ before ┆ t_a ┆ t_b ┆ t_c ┆ t_d ┆ after │ │ --- ┆ --- ┆ --- ┆ --- ┆ --- ┆ --- │ │ str ┆ i64 ┆ str ┆ bool ┆ list[i64] ┆ str │ ╞════════╪═════╪═════╪══════╪═══════════╪═══════╡ │ foo ┆ 1 ┆ a ┆ true ┆ [1, 2] ┆ baz │ │ bar ┆ 2 ┆ b ┆ null ┆ [3] ┆ womp │ └────────┴─────┴─────┴──────┴───────────┴───────┘ Unnest all struct columns by calling without arguments: >>> df.unnest() shape: (2, 6) ┌────────┬─────┬─────┬──────┬───────────┬───────┐ │ before ┆ t_a ┆ t_b ┆ t_c ┆ t_d ┆ after │ │ --- ┆ --- ┆ --- ┆ --- ┆ --- ┆ --- │ │ str ┆ i64 ┆ str ┆ bool ┆ list[i64] ┆ str │ ╞════════╪═════╪═════╪══════╪═══════════╪═══════╡ │ foo ┆ 1 ┆ a ┆ true ┆ [1, 2] ┆ baz │ │ bar ┆ 2 ┆ b ┆ null ┆ [3] ┆ womp │ └────────┴─────┴─────┴──────┴───────────┴───────┘ >>> df = pl.DataFrame( ... { ... "before": ["foo", "bar"], ... "t_a": [1, 2], ... "t_b": ["a", "b"], ... "t_c": [True, None], ... "t_d": [[1, 2], [3]], ... "after": ["baz", "womp"], ... } ... ).select( ... "before", ... pl.struct(pl.col("^t_.$").name.map(lambda t: t[2:])).alias("t"), ... "after", ... ) >>> df.unnest("t", separator="::") shape: (2, 6) ┌────────┬──────┬──────┬──────┬───────────┬───────┐ │ before ┆ t::a ┆ t::b ┆ t::c ┆ t::d ┆ after │ │ --- ┆ --- ┆ --- ┆ --- ┆ --- ┆ --- │ │ str ┆ i64 ┆ str ┆ bool ┆ list[i64] ┆ str │ ╞════════╪══════╪══════╪══════╪═══════════╪═══════╡ │ foo ┆ 1 ┆ a ┆ true ┆ [1, 2] ┆ baz │ │ bar ┆ 2 ┆ b ┆ null ┆ [3] ┆ womp │ └────────┴──────┴──────┴──────┴───────────┴───────┘
 [\[source\]](https://github.com/pola-rs/polars/blob/py-1.43.2/py-polars/src/../src/polars/dataframe/frame.py#L12490-L12591)  - unpivot(
  - *on: ColumnNameOrSelector | Sequence[ColumnNameOrSelector] | [None](https://docs.python.org/3/library/constants.html#None) = None* ,
  - *** ,
  - *index: ColumnNameOrSelector | Sequence[ColumnNameOrSelector] | [None](https://docs.python.org/3/library/constants.html#None) = None* ,
  - *variable_name: [str](https://docs.python.org/3/library/stdtypes.html#str) | [None](https://docs.python.org/3/library/constants.html#None) = None* ,
  - *value_name: [str](https://docs.python.org/3/library/stdtypes.html#str) | [None](https://docs.python.org/3/library/constants.html#None) = None* ,
  - Unpivot a DataFrame from wide to long format. Optionally leaves identifiers set. This function is useful to massage a DataFrame into a format where one or more columns are identifier variables (index) while all other columns, considered measured variables (on), are “unpivoted” to the row axis leaving just two non-identifier columns, ‘variable’ and ‘value’. 
    - Parameters:
      - **on**
      - Column(s) or selector(s) to use as values variables; if `on` is empty no columns will be used. If set to`None` (default)
all columns that are not in`index` will be used.
      - **index**
      - Column(s) or selector(s) to use as identifier variables.
      - **variable_name**
      - Name to give to the `variable` column. Defaults to “variable”
      - **value_name**
      - Name to give to the `value` column. Defaults to “value”
 Notes If you’re coming from pandas, this is similar to `pandas.DataFrame.melt` ,
but with`index` replacing`id_vars` and`on` replacing`value_vars` .
In other frameworks, you might know this operation as`pivot_longer` .The resulting row order is unspecified. Examples >>> df = pl.DataFrame( ... { ... "a": ["x", "y", "z"], ... "b": [1, 3, 5], ... "c": [2, 4, 6], ... } ... ) >>> import polars.selectors as cs >>> df.unpivot(cs.numeric(), index="a") shape: (6, 3) ┌─────┬──────────┬───────┐ │ a ┆ variable ┆ value │ │ --- ┆ --- ┆ --- │ │ str ┆ str ┆ i64 │ ╞═════╪══════════╪═══════╡ │ x ┆ b ┆ 1 │ │ y ┆ b ┆ 3 │ │ z ┆ b ┆ 5 │ │ x ┆ c ┆ 2 │ │ y ┆ c ┆ 4 │ │ z ┆ c ┆ 6 │ └─────┴──────────┴───────┘
 [\[source\]](https://github.com/pola-rs/polars/blob/py-1.43.2/py-polars/src/../src/polars/dataframe/frame.py#L9824-L9893)  - unstack(
  - *** ,
  - *step: [int](https://docs.python.org/3/library/functions.html#int)* ,
  - *how: UnstackDirection = 'vertical'* ,
  - *columns: ColumnNameOrSelector | Sequence[ColumnNameOrSelector] | [None](https://docs.python.org/3/library/constants.html#None) = None* ,
  - *fill_values: [list](https://docs.python.org/3/library/stdtypes.html#list)[Any] | [None](https://docs.python.org/3/library/constants.html#None) = None* ,
  - Unstack a long table to a wide form without doing an aggregation. This can be much faster than a pivot, because it can skip the grouping phase. 
    - Parameters:
      - **step**
      - Number of rows in the unstacked frame.
      - **how** { ‘vertical’, ‘horizontal’ }
      - Direction of the unstack.
      - **columns**
      - Column name(s) or selector(s) to include in the operation. If set to `None` (default), use all columns.
      - **fill_values**
      - Fill values that don’t fit the new size with this value.
 Examples >>> from string import ascii_uppercase >>> df = pl.DataFrame( ... { ... "x": list(ascii_uppercase[0:8]), ... "y": pl.int_range(1, 9, eager=True), ... } ... ).with_columns( ... z=pl.int_ranges(pl.col("y"), pl.col("y") + 2, dtype=pl.UInt8), ... ) >>> df shape: (8, 3) ┌─────┬─────┬──────────┐ │ x ┆ y ┆ z │ │ --- ┆ --- ┆ --- │ │ str ┆ i64 ┆ list[u8] │ ╞═════╪═════╪══════════╡ │ A ┆ 1 ┆ [1, 2] │ │ B ┆ 2 ┆ [2, 3] │ │ C ┆ 3 ┆ [3, 4] │ │ D ┆ 4 ┆ [4, 5] │ │ E ┆ 5 ┆ [5, 6] │ │ F ┆ 6 ┆ [6, 7] │ │ G ┆ 7 ┆ [7, 8] │ │ H ┆ 8 ┆ [8, 9] │ └─────┴─────┴──────────┘ >>> df.unstack(step=4, how="vertical") shape: (4, 6) ┌─────┬─────┬─────┬─────┬──────────┬──────────┐ │ x_0 ┆ x_1 ┆ y_0 ┆ y_1 ┆ z_0 ┆ z_1 │ │ --- ┆ --- ┆ --- ┆ --- ┆ --- ┆ --- │ │ str ┆ str ┆ i64 ┆ i64 ┆ list[u8] ┆ list[u8] │ ╞═════╪═════╪═════╪═════╪══════════╪══════════╡ │ A ┆ E ┆ 1 ┆ 5 ┆ [1, 2] ┆ [5, 6] │ │ B ┆ F ┆ 2 ┆ 6 ┆ [2, 3] ┆ [6, 7] │ │ C ┆ G ┆ 3 ┆ 7 ┆ [3, 4] ┆ [7, 8] │ │ D ┆ H ┆ 4 ┆ 8 ┆ [4, 5] ┆ [8, 9] │ └─────┴─────┴─────┴─────┴──────────┴──────────┘ >>> df.unstack(step=2, how="horizontal") shape: (4, 6) ┌─────┬─────┬─────┬─────┬──────────┬──────────┐ │ x_0 ┆ x_1 ┆ y_0 ┆ y_1 ┆ z_0 ┆ z_1 │ │ --- ┆ --- ┆ --- ┆ --- ┆ --- ┆ --- │ │ str ┆ str ┆ i64 ┆ i64 ┆ list[u8] ┆ list[u8] │ ╞═════╪═════╪═════╪═════╪══════════╪══════════╡ │ A ┆ B ┆ 1 ┆ 2 ┆ [1, 2] ┆ [2, 3] │ │ C ┆ D ┆ 3 ┆ 4 ┆ [3, 4] ┆ [4, 5] │ │ E ┆ F ┆ 5 ┆ 6 ┆ [5, 6] ┆ [6, 7] │ │ G ┆ H ┆ 7 ┆ 8 ┆ [7, 8] ┆ [8, 9] │ └─────┴─────┴─────┴─────┴──────────┴──────────┘ >>> import polars.selectors as cs >>> df.unstack(step=5, columns=cs.numeric(), fill_values=0) shape: (5, 2) ┌─────┬─────┐ │ y_0 ┆ y_1 │ │ --- ┆ --- │ │ i64 ┆ i64 │ ╞═════╪═════╡ │ 1 ┆ 6 │ │ 2 ┆ 7 │ │ 3 ┆ 8 │ │ 4 ┆ 0 │ │ 5 ┆ 0 │ └─────┴─────┘
 [\[source\]](https://github.com/pola-rs/polars/blob/py-1.43.2/py-polars/src/../src/polars/dataframe/frame.py#L9895-L10027)  - update(
  - *other: DataFrame* ,
  - *on: [str](https://docs.python.org/3/library/stdtypes.html#str) | Sequence[[str](https://docs.python.org/3/library/stdtypes.html#str)] | [None](https://docs.python.org/3/library/constants.html#None) = None* ,
  - *how: Literal['left', 'inner', 'full'] = 'left'* ,
  - *** ,
  - *left_on: [str](https://docs.python.org/3/library/stdtypes.html#str) | Sequence[[str](https://docs.python.org/3/library/stdtypes.html#str)] | [None](https://docs.python.org/3/library/constants.html#None) = None* ,
  - *right_on: [str](https://docs.python.org/3/library/stdtypes.html#str) | Sequence[[str](https://docs.python.org/3/library/stdtypes.html#str)] | [None](https://docs.python.org/3/library/constants.html#None) = None* ,
  - *include_nulls: [bool](https://docs.python.org/3/library/functions.html#bool) = False* ,
  - *maintain_order: MaintainOrderJoin | [None](https://docs.python.org/3/library/constants.html#None) = 'left'* ,
  - Update the values in this `DataFrame` with the values in`other` .Warning This functionality is considered **unstable** . It may be changed
at any point without it being considered a breaking change.
    - Parameters:
      - **other**
      - DataFrame that will be used to update the values
      - **on**
      - Column names that will be joined on. If set to `None` (default),
the implicit row index of each frame is used as a join key.
      - **how** {‘left’, ‘inner’, ‘full’}
        - ‘left’ will keep all rows from the left table; rows may be duplicated if multiple rows in the right frame match the left row’s key.
        - ‘inner’ keeps only those rows where the key exists in both frames.
        - ‘full’ will update existing rows where the key matches while also adding any new rows contained in the given frame.
      - **left_on**
      - Join column(s) of the left DataFrame.
      - **right_on**
      - Join column(s) of the right DataFrame.
      - **include_nulls**
      - Overwrite values in the left frame with null values from the right frame. If set to `False` (default), null values in the right frame are ignored.
      - **maintain_order** {‘none’, ‘left’, ‘right’, ‘left_right’, ‘right_left’}
      - Which order of rows from the inputs to preserve. See [`join()`](api/polars.DataFrame.join.html#polars.DataFrame.join) for details. Unlike`join` this function preserves the left order by
default.
 Notes This is syntactic sugar for a left/inner join that preserves the order of the left `DataFrame` by default, with an optional coalesce when`include_nulls = False` .Examples >>> df = pl.DataFrame( ... { ... "A": [1, 2, 3, 4], ... "B": [400, 500, 600, 700], ... } ... ) >>> df shape: (4, 2) ┌─────┬─────┐ │ A ┆ B │ │ --- ┆ --- │ │ i64 ┆ i64 │ ╞═════╪═════╡ │ 1 ┆ 400 │ │ 2 ┆ 500 │ │ 3 ┆ 600 │ │ 4 ┆ 700 │ └─────┴─────┘ >>> new_df = pl.DataFrame( ... { ... "B": [-66, None, -99], ... "C": [5, 3, 1], ... } ... ) Update `df` values with the non-null values in`new_df` , by row index:>>> df.update(new_df) shape: (4, 2) ┌─────┬─────┐ │ A ┆ B │ │ --- ┆ --- │ │ i64 ┆ i64 │ ╞═════╪═════╡ │ 1 ┆ -66 │ │ 2 ┆ 500 │ │ 3 ┆ -99 │ │ 4 ┆ 700 │ └─────┴─────┘ Update `df` values with the non-null values in`new_df` , by row index,
but only keeping those rows that are common to both frames:>>> df.update(new_df, how="inner") shape: (3, 2) ┌─────┬─────┐ │ A ┆ B │ │ --- ┆ --- │ │ i64 ┆ i64 │ ╞═════╪═════╡ │ 1 ┆ -66 │ │ 2 ┆ 500 │ │ 3 ┆ -99 │ └─────┴─────┘ Update `df` values with the non-null values in`new_df` , using a full
outer join strategy that defines explicit join columns in each frame:>>> df.update(new_df, left_on=["A"], right_on=["C"], how="full") shape: (5, 2) ┌─────┬─────┐ │ A ┆ B │ │ --- ┆ --- │ │ i64 ┆ i64 │ ╞═════╪═════╡ │ 1 ┆ -99 │ │ 2 ┆ 500 │ │ 3 ┆ 600 │ │ 4 ┆ 700 │ │ 5 ┆ -66 │ └─────┴─────┘ Update `df` values including null values in`new_df` , using a full outer
join strategy that defines explicit join columns in each frame:>>> df.update(new_df, left_on="A", right_on="C", how="full", include_nulls=True) shape: (5, 2) ┌─────┬──────┐ │ A ┆ B │ │ --- ┆ --- │ │ i64 ┆ i64 │ ╞═════╪══════╡ │ 1 ┆ -99 │ │ 2 ┆ 500 │ │ 3 ┆ null │ │ 4 ┆ 700 │ │ 5 ┆ -66 │ └─────┴──────┘
 [\[source\]](https://github.com/pola-rs/polars/blob/py-1.43.2/py-polars/src/../src/polars/dataframe/frame.py#L12800-L12956)  - upsample(
  - *time_column: [str](https://docs.python.org/3/library/stdtypes.html#str)* ,
  - *** ,
  - *every: [str](https://docs.python.org/3/library/stdtypes.html#str) | timedelta* ,
  - *group_by: [str](https://docs.python.org/3/library/stdtypes.html#str) | Sequence[[str](https://docs.python.org/3/library/stdtypes.html#str)] | [None](https://docs.python.org/3/library/constants.html#None) = None* ,
  - *maintain_order: [bool](https://docs.python.org/3/library/functions.html#bool) = False* ,
  - Upsample a DataFrame at a regular frequency. The `every` argument is created with the following string language:
    - 1ns (1 nanosecond)
    - 1us (1 microsecond)
    - 1ms (1 millisecond)
    - 1s (1 second)
    - 1m (1 minute)
    - 1h (1 hour)
    - 1d (1 calendar day)
    - 1w (1 calendar week)
    - 1mo (1 calendar month)
    - 1q (1 calendar quarter)
    - 1y (1 calendar year)
    - 1i (1 index count)
 Or combine them: 
    - “3d12h4m25s” # 3 days, 12 hours, 4 minutes, and 25 seconds
 By “calendar day”, we mean the corresponding time on the next day (which may not be 24 hours, due to daylight savings). Similarly for “calendar week”, “calendar month”, “calendar quarter”, and “calendar year”. Changed in version 0.20.14: The `by` parameter was renamed`group_by` .
    - Parameters:
      - **time_column**
      - Time column will be used to determine a date_range. Note that this column has to be sorted for the output to make sense.
      - **every**
      - Interval will start ‘every’ duration.
      - **group_by**
      - First group by these columns and then upsample for every group.
      - **maintain_order**
      - Keep the ordering predictable. This is slower.
    - Returns:
      - DataFrame
      - Result will be sorted by `time_column` (but note that if`group_by` columns
are passed, it will only be sorted within each group).
 Examples Upsample a DataFrame by a certain interval. >>> from datetime import datetime >>> df = pl.DataFrame( ... { ... "time": [ ... datetime(2021, 2, 1), ... datetime(2021, 4, 1), ... datetime(2021, 5, 1), ... datetime(2021, 6, 1), ... ], ... "groups": ["A", "B", "A", "B"], ... "values": [0, 1, 2, 3], ... } ... ).set_sorted("time") >>> df.upsample( ... time_column="time", every="1mo", group_by="groups", maintain_order=True ... ).select(pl.all().fill_null(strategy="forward")) shape: (7, 3) ┌─────────────────────┬────────┬────────┐ │ time ┆ groups ┆ values │ │ --- ┆ --- ┆ --- │ │ datetime[μs] ┆ str ┆ i64 │ ╞═════════════════════╪════════╪════════╡ │ 2021-02-01 00:00:00 ┆ A ┆ 0 │ │ 2021-03-01 00:00:00 ┆ A ┆ 0 │ │ 2021-04-01 00:00:00 ┆ A ┆ 0 │ │ 2021-05-01 00:00:00 ┆ A ┆ 2 │ │ 2021-04-01 00:00:00 ┆ B ┆ 1 │ │ 2021-05-01 00:00:00 ┆ B ┆ 1 │ │ 2021-06-01 00:00:00 ┆ B ┆ 3 │ └─────────────────────┴────────┴────────┘
 [\[source\]](https://github.com/pola-rs/polars/blob/py-1.43.2/py-polars/src/../src/polars/dataframe/frame.py#L7793-L7893)  - 
var(*ddof: [int](https://docs.python.org/3/library/functions.html#int) = 1* )  DataFrame[\[source\]](https://github.com/pola-rs/polars/blob/py-1.43.2/py-polars/src/../src/polars/dataframe/frame.py#L11067-L11108)
  - Aggregate the columns of this DataFrame to their variance value. 
    - Parameters:
      - **ddof**
      - “Delta Degrees of Freedom”: the divisor used in the calculation is N - ddof, where N represents the number of elements. By default ddof is 1.
 Examples >>> df = pl.DataFrame( ... { ... "foo": [1, 2, 3], ... "bar": [6, 7, 8], ... "ham": ["a", "b", "c"], ... } ... ) >>> df.var() shape: (1, 3) ┌─────┬─────┬──────┐ │ foo ┆ bar ┆ ham │ │ --- ┆ --- ┆ --- │ │ f64 ┆ f64 ┆ str │ ╞═════╪═════╪══════╡ │ 1.0 ┆ 1.0 ┆ null │ └─────┴─────┴──────┘ >>> df.var(ddof=0) shape: (1, 3) ┌──────────┬──────────┬──────┐ │ foo ┆ bar ┆ ham │ │ --- ┆ --- ┆ --- │ │ f64 ┆ f64 ┆ str │ ╞══════════╪══════════╪══════╡ │ 0.666667 ┆ 0.666667 ┆ null │ └──────────┴──────────┴──────┘
 
  - vstack(
  - *other: DataFrame* ,
  - *** ,
  - *in_place: [bool](https://docs.python.org/3/library/functions.html#bool) = False* ,
  - Grow this DataFrame vertically by stacking a DataFrame to it. 
    - Parameters:
      - **other**
      - DataFrame to stack.
      - **in_place**
      - Modify in place.
 See also Examples >>> df1 = pl.DataFrame( ... { ... "foo": [1, 2], ... "bar": [6, 7], ... "ham": ["a", "b"], ... } ... ) >>> df2 = pl.DataFrame( ... { ... "foo": [3, 4], ... "bar": [8, 9], ... "ham": ["c", "d"], ... } ... ) >>> df1.vstack(df2) shape: (4, 3) ┌─────┬─────┬─────┐ │ foo ┆ bar ┆ ham │ │ --- ┆ --- ┆ --- │ │ i64 ┆ i64 ┆ str │ ╞═════╪═════╪═════╡ │ 1 ┆ 6 ┆ a │ │ 2 ┆ 7 ┆ b │ │ 3 ┆ 8 ┆ c │ │ 4 ┆ 9 ┆ d │ └─────┴─────┴─────┘
 [\[source\]](https://github.com/pola-rs/polars/blob/py-1.43.2/py-polars/src/../src/polars/dataframe/frame.py#L8831-L8880)  - 
*property* width*: [int](https://docs.python.org/3/library/functions.html#int)*[\[source\]](https://github.com/pola-rs/polars/blob/py-1.43.2/py-polars/src/../src/polars/dataframe/frame.py#L854-L874)
  - Get the number of columns. 
    - Returns:
      - int
 Examples >>> df = pl.DataFrame( ... { ... "foo": [1, 2, 3], ... "bar": [4, 5, 6], ... } ... ) >>> df.width 2
 
  - with_columns(
  - **exprs: IntoExpr | Iterable[IntoExpr]* ,
  - ***named_exprs: IntoExpr* ,
  - Add columns to this DataFrame. Added columns will replace existing columns with the same name. 
    - Parameters:
      - ***exprs**
      - Column(s) to add, specified as positional arguments. Accepts expression input. Strings are parsed as column names, other non-expression inputs are parsed as literals.
      - ****named_exprs**
      - Additional columns to add, specified as keyword arguments. The columns will be renamed to the keyword used.
    - Returns:
      - DataFrame
      - A new DataFrame with the columns added.
 Notes Creating a new DataFrame using this method does not create a new copy of existing data. Examples Pass an expression to add it as a new column. >>> df = pl.DataFrame( ... { ... "a": [1, 2, 3, 4], ... "b": [0.5, 4, 10, 13], ... "c": [True, True, False, True], ... } ... ) >>> df.with_columns((pl.col("a") ** 2).alias("a^2")) shape: (4, 4) ┌─────┬──────┬───────┬─────┐ │ a ┆ b ┆ c ┆ a^2 │ │ --- ┆ --- ┆ --- ┆ --- │ │ i64 ┆ f64 ┆ bool ┆ i64 │ ╞═════╪══════╪═══════╪═════╡ │ 1 ┆ 0.5 ┆ true ┆ 1 │ │ 2 ┆ 4.0 ┆ true ┆ 4 │ │ 3 ┆ 10.0 ┆ false ┆ 9 │ │ 4 ┆ 13.0 ┆ true ┆ 16 │ └─────┴──────┴───────┴─────┘ Added columns will replace existing columns with the same name. >>> df.with_columns(pl.col("a").cast(pl.Float64)) shape: (4, 3) ┌─────┬──────┬───────┐ │ a ┆ b ┆ c │ │ --- ┆ --- ┆ --- │ │ f64 ┆ f64 ┆ bool │ ╞═════╪══════╪═══════╡ │ 1.0 ┆ 0.5 ┆ true │ │ 2.0 ┆ 4.0 ┆ true │ │ 3.0 ┆ 10.0 ┆ false │ │ 4.0 ┆ 13.0 ┆ true │ └─────┴──────┴───────┘ Multiple columns can be added using positional arguments. >>> df.with_columns( ... (pl.col("a") ** 2).alias("a^2"), ... (pl.col("b") / 2).alias("b/2"), ... (pl.col("c").not_()).alias("not c"), ... ) shape: (4, 6) ┌─────┬──────┬───────┬─────┬──────┬───────┐ │ a ┆ b ┆ c ┆ a^2 ┆ b/2 ┆ not c │ │ --- ┆ --- ┆ --- ┆ --- ┆ --- ┆ --- │ │ i64 ┆ f64 ┆ bool ┆ i64 ┆ f64 ┆ bool │ ╞═════╪══════╪═══════╪═════╪══════╪═══════╡ │ 1 ┆ 0.5 ┆ true ┆ 1 ┆ 0.25 ┆ false │ │ 2 ┆ 4.0 ┆ true ┆ 4 ┆ 2.0 ┆ false │ │ 3 ┆ 10.0 ┆ false ┆ 9 ┆ 5.0 ┆ true │ │ 4 ┆ 13.0 ┆ true ┆ 16 ┆ 6.5 ┆ false │ └─────┴──────┴───────┴─────┴──────┴───────┘ Multiple columns can also be added by passing a list of expressions. >>> df.with_columns( ... [ ... (pl.col("a") ** 2).alias("a^2"), ... (pl.col("b") / 2).alias("b/2"), ... (pl.col("c").not_()).alias("not c"), ... ] ... ) shape: (4, 6) ┌─────┬──────┬───────┬─────┬──────┬───────┐ │ a ┆ b ┆ c ┆ a^2 ┆ b/2 ┆ not c │ │ --- ┆ --- ┆ --- ┆ --- ┆ --- ┆ --- │ │ i64 ┆ f64 ┆ bool ┆ i64 ┆ f64 ┆ bool │ ╞═════╪══════╪═══════╪═════╪══════╪═══════╡ │ 1 ┆ 0.5 ┆ true ┆ 1 ┆ 0.25 ┆ false │ │ 2 ┆ 4.0 ┆ true ┆ 4 ┆ 2.0 ┆ false │ │ 3 ┆ 10.0 ┆ false ┆ 9 ┆ 5.0 ┆ true │ │ 4 ┆ 13.0 ┆ true ┆ 16 ┆ 6.5 ┆ false │ └─────┴──────┴───────┴─────┴──────┴───────┘ Use keyword arguments to easily name your expression inputs. >>> df.with_columns( ... ab=pl.col("a") * pl.col("b"), ... not_c=pl.col("c").not_(), ... ) shape: (4, 5) ┌─────┬──────┬───────┬──────┬───────┐ │ a ┆ b ┆ c ┆ ab ┆ not_c │ │ --- ┆ --- ┆ --- ┆ --- ┆ --- │ │ i64 ┆ f64 ┆ bool ┆ f64 ┆ bool │ ╞═════╪══════╪═══════╪══════╪═══════╡ │ 1 ┆ 0.5 ┆ true ┆ 0.5 ┆ false │ │ 2 ┆ 4.0 ┆ true ┆ 8.0 ┆ false │ │ 3 ┆ 10.0 ┆ false ┆ 30.0 ┆ true │ │ 4 ┆ 13.0 ┆ true ┆ 52.0 ┆ false │ └─────┴──────┴───────┴──────┴───────┘
 [\[source\]](https://github.com/pola-rs/polars/blob/py-1.43.2/py-polars/src/../src/polars/dataframe/frame.py#L10570-L10703)  - with_columns_seq(
  - **exprs: IntoExpr | Iterable[IntoExpr]* ,
  - ***named_exprs: IntoExpr* ,
  - Add columns to this DataFrame. Added columns will replace existing columns with the same name. This will run all expression sequentially instead of in parallel. Use this when the work per expression is cheap. 
    - Parameters:
      - ***exprs**
      - Column(s) to add, specified as positional arguments. Accepts expression input. Strings are parsed as column names, other non-expression inputs are parsed as literals.
      - ****named_exprs**
      - Additional columns to add, specified as keyword arguments. The columns will be renamed to the keyword used.
    - Returns:
      - DataFrame
      - A new DataFrame with the columns added.
 See also
 [\[source\]](https://github.com/pola-rs/polars/blob/py-1.43.2/py-polars/src/../src/polars/dataframe/frame.py#L10705-L10743)  - with_row_count( ) DataFrame
  - Add a column at index 0 that counts the rows. Deprecated since version 0.20.4: Use the [`with_row_index()`](api/polars.DataFrame.with_row_index.html#polars.DataFrame.with_row_index) method instead.
Note that the default column name has changed from ‘row_nr’ to ‘index’.
    - Parameters:
      - **name**
      - Name of the column to add.
      - **offset**
      - Start the row count at this offset. Default = 0
 Examples >>> df = pl.DataFrame( ... { ... "a": [1, 3, 5], ... "b": [2, 4, 6], ... } ... ) >>> df.with_row_count() shape: (3, 3) ┌────────┬─────┬─────┐ │ row_nr ┆ a ┆ b │ │ --- ┆ --- ┆ --- │ │ u32 ┆ i64 ┆ i64 │ ╞════════╪═════╪═════╡ │ 0 ┆ 1 ┆ 2 │ │ 1 ┆ 3 ┆ 4 │ │ 2 ┆ 5 ┆ 6 │ └────────┴─────┴─────┘
 [\[source\]](https://github.com/pola-rs/polars/blob/py-1.43.2/py-polars/src/../src/polars/dataframe/frame.py#L7122-L7161)  - with_row_index( ) DataFrame
  - Add a row index as the first column in the DataFrame. 
    - Parameters:
      - **name**
      - Name of the index column.
      - **offset**
      - Start the index at this offset. Cannot be negative.
 Notes The resulting column does not have any special properties. It is a regular column of type `UInt32` (or`UInt64` in`polars[rt64]` ).Examples >>> df = pl.DataFrame( ... { ... "a": [1, 3, 5], ... "b": [2, 4, 6], ... } ... ) >>> df.with_row_index() shape: (3, 3) ┌───────┬─────┬─────┐ │ index ┆ a ┆ b │ │ --- ┆ --- ┆ --- │ │ u32 ┆ i64 ┆ i64 │ ╞═══════╪═════╪═════╡ │ 0 ┆ 1 ┆ 2 │ │ 1 ┆ 3 ┆ 4 │ │ 2 ┆ 5 ┆ 6 │ └───────┴─────┴─────┘ >>> df.with_row_index("id", offset=1000) shape: (3, 3) ┌──────┬─────┬─────┐ │ id ┆ a ┆ b │ │ --- ┆ --- ┆ --- │ │ u32 ┆ i64 ┆ i64 │ ╞══════╪═════╪═════╡ │ 1000 ┆ 1 ┆ 2 │ │ 1001 ┆ 3 ┆ 4 │ │ 1002 ┆ 5 ┆ 6 │ └──────┴─────┴─────┘ An index column can also be created using the expressions [`int_range()`](../expressions/api/polars.int_range.html#polars.int_range) and[`len()`](../expressions/api/polars.len.html#polars.len) .>>> df.select( ... pl.int_range(pl.len(), dtype=pl.UInt32).alias("index"), ... pl.all(), ... ) shape: (3, 3) ┌───────┬─────┬─────┐ │ index ┆ a ┆ b │ │ --- ┆ --- ┆ --- │ │ u32 ┆ i64 ┆ i64 │ ╞═══════╪═════╪═════╡ │ 0 ┆ 1 ┆ 2 │ │ 1 ┆ 3 ┆ 4 │ │ 2 ┆ 5 ┆ 6 │ └───────┴─────┴─────┘
 [\[source\]](https://github.com/pola-rs/polars/blob/py-1.43.2/py-polars/src/../src/polars/dataframe/frame.py#L7050-L7120)  - write_avro( )
  - Write to Apache Avro file. 
    - Parameters:
      - **file**
      - File path or writable file-like object to which the data will be written.
      - **compression** {‘uncompressed’, ‘snappy’, ‘deflate’}
      - Compression method. Defaults to “uncompressed”.
      - **name**
      - Schema name. Defaults to empty string.
 Examples >>> import pathlib >>> >>> df = pl.DataFrame( ... { ... "foo": [1, 2, 3, 4, 5], ... "bar": [6, 7, 8, 9, 10], ... "ham": ["a", "b", "c", "d", "e"], ... } ... ) >>> path: pathlib.Path = dirpath / "new_file.avro" >>> df.write_avro(path)
 [None](https://docs.python.org/3/library/constants.html#None)[\[source\]](https://github.com/pola-rs/polars/blob/py-1.43.2/py-polars/src/../src/polars/dataframe/frame.py#L3293-L3332)  - 
write_clipboard(*** ,*separator: [str](https://docs.python.org/3/library/stdtypes.html#str) = '\t'* ,***kwargs: [Any](https://docs.python.org/3/library/typing.html#typing.Any)* )[None](https://docs.python.org/3/library/constants.html#None)[\[source\]](https://github.com/pola-rs/polars/blob/py-1.43.2/py-polars/src/../src/polars/dataframe/frame.py#L3272-L3291)
  - Copy `DataFrame` in csv format to the system clipboard with`write_csv` .Useful for pasting into Excel or other similar spreadsheet software. 
    - Parameters:
      - **separator**
      - Separate CSV fields with this symbol.
      - **kwargs**
      - Additional arguments to pass to `write_csv` .
 See also 
    - [`polars.read_clipboard`](../api/polars.read_clipboard.html#polars.read_clipboard)
    - Read a DataFrame from the clipboard.
    - [`write_csv`](../api/polars.DataFrame.write_csv.html#polars.DataFrame.write_csv)
    - Write to comma-separated values (CSV) file.
 
  - write_csv(
  - *file: [str](https://docs.python.org/3/library/stdtypes.html#str) | Path | IO[[str](https://docs.python.org/3/library/stdtypes.html#str)] | IO[[bytes](https://docs.python.org/3/library/stdtypes.html#bytes)] | [None](https://docs.python.org/3/library/constants.html#None) = None* ,
  - *** ,
  - *include_bom: [bool](https://docs.python.org/3/library/functions.html#bool) = False* ,
  - *compression: Literal['uncompressed', 'gzip', 'zstd'] = 'uncompressed'* ,
  - *compression_level: [int](https://docs.python.org/3/library/functions.html#int) | [None](https://docs.python.org/3/library/constants.html#None) = None* ,
  - *check_extension: [bool](https://docs.python.org/3/library/functions.html#bool) = True* ,
  - *include_header: [bool](https://docs.python.org/3/library/functions.html#bool) = True* ,
  - *separator: [str](https://docs.python.org/3/library/stdtypes.html#str) = ','* ,
  - *line_terminator: [str](https://docs.python.org/3/library/stdtypes.html#str) = '\n'* ,
  - *quote_char: [str](https://docs.python.org/3/library/stdtypes.html#str) = '"'* ,
  - *batch_size: [int](https://docs.python.org/3/library/functions.html#int) = 1024* ,
  - *datetime_format: [str](https://docs.python.org/3/library/stdtypes.html#str) | [None](https://docs.python.org/3/library/constants.html#None) = None* ,
  - *date_format: [str](https://docs.python.org/3/library/stdtypes.html#str) | [None](https://docs.python.org/3/library/constants.html#None) = None* ,
  - *time_format: [str](https://docs.python.org/3/library/stdtypes.html#str) | [None](https://docs.python.org/3/library/constants.html#None) = None* ,
  - *float_scientific: [bool](https://docs.python.org/3/library/functions.html#bool) | [None](https://docs.python.org/3/library/constants.html#None) = None* ,
  - *float_precision: [int](https://docs.python.org/3/library/functions.html#int) | [None](https://docs.python.org/3/library/constants.html#None) = None* ,
  - *decimal_comma: [bool](https://docs.python.org/3/library/functions.html#bool) = False* ,
  - *null_value: [str](https://docs.python.org/3/library/stdtypes.html#str) | [None](https://docs.python.org/3/library/constants.html#None) = None* ,
  - *quote_style: CsvQuoteStyle | [None](https://docs.python.org/3/library/constants.html#None) = None* ,
  - *storage_options: StorageOptionsDict | [None](https://docs.python.org/3/library/constants.html#None) = None* ,
  - *credential_provider: CredentialProviderFunction | Literal['auto'] | [None](https://docs.python.org/3/library/constants.html#None) = 'auto'* ,
  - *retries: [int](https://docs.python.org/3/library/functions.html#int) | [None](https://docs.python.org/3/library/constants.html#None) = None* ,
  - Write to comma-separated values (CSV) file. 
    - Parameters:
      - **file**
      - File path or writable file-like object to which the result will be written. If set to `None` (default), the output is returned as a string instead.
      - **include_bom**
      - Whether to include UTF-8 BOM in the CSV output.
      - **compression**
      - What compression format to use. Warning This functionality is considered **unstable** . It may be changed at any
point without it being considered a breaking change.
      - **compression_level**
      - The compression level to use, typically 0-9 or `None` to let the
engine choose.Warning This functionality is considered **unstable** . It may be changed at any
point without it being considered a breaking change.
      - **check_extension**
      - Whether to check if the filename matches the compression settings. Will raise an error if compression is set to ‘uncompressed’ and the filename ends in one of (“.gz”, “.zst”, “.zstd”) or if compression != ‘uncompressed’ and the filename does not end in the appropriate extension. Only applies if file is a path. Warning This functionality is considered **unstable** . It may be changed at any
point without it being considered a breaking change.
      - **include_header**
      - Whether to include header in the CSV output.
      - **separator**
      - Separate CSV fields with this symbol.
      - **line_terminator**
      - String used to end each row.
      - **quote_char**
      - Byte to use as quoting character.
      - **batch_size**
      - Number of rows that will be processed per thread.
      - **datetime_format**
      - A format string, with the specifiers defined by the [chrono](https://docs.rs/chrono/latest/chrono/format/strftime/index.html) Rust crate. If no format specified, the default fractional-second
precision is inferred from the maximum timeunit found in the frame’s
Datetime cols (if any).
      - **date_format**
      - A format string, with the specifiers defined by the [chrono](https://docs.rs/chrono/latest/chrono/format/strftime/index.html) Rust crate.
      - **time_format**
      - A format string, with the specifiers defined by the [chrono](https://docs.rs/chrono/latest/chrono/format/strftime/index.html) Rust crate.
      - **float_scientific**
      - Whether to use scientific form always (true), never (false), or automatically (None) for floating-point datatypes.
      - **float_precision**
      - Number of decimal places to write, applied to both floating-point data types.
      - **decimal_comma**
      - Use a comma as the decimal separator instead of a point in standard notation. Floats will be encapsulated in quotes if necessary; set the field separator to override.
      - **null_value**
      - A string representing null values (defaulting to the empty string).
      - **quote_style** {‘necessary’, ‘always’, ‘non_numeric’, ‘never’}
      - Determines the quoting strategy used. 
        - necessary (default): This puts quotes around fields only when necessary. They are necessary when fields contain a quote, separator or record terminator. Quotes are also necessary when writing an empty record (which is indistinguishable from a record with one empty field). This is the default.
        - always: This puts quotes around every field. Always.
        - never: This never puts quotes around fields, even if that results in invalid CSV data (e.g.: by not quoting strings containing the separator).
        - non_numeric: This puts quotes around all fields that are non-numeric. Namely, when writing a field that does not parse as a valid float or integer, then quotes will be used even if they aren`t strictly necessary.
      - **storage_options**
      - Options that indicate how to connect to a cloud provider. The cloud providers currently supported are AWS, GCP, and Azure. See supported keys here: 
        - [aws](https://docs.rs/object_store/latest/object_store/aws/enum.AmazonS3ConfigKey.html)
        - [gcp](https://docs.rs/object_store/latest/object_store/gcp/enum.GoogleConfigKey.html)
        - [azure](https://docs.rs/object_store/latest/object_store/azure/enum.AzureConfigKey.html)
        - Hugging Face ( `hf://` ): Accepts an API key under the`token` parameter:`{'token': '...'}` , or by setting the`HF_TOKEN` environment variable.
 If `storage_options` is not provided, Polars will try to infer the
information from environment variables.
      - **credential_provider**
      - Provide a function that can be called to provide cloud storage credentials. The function is expected to return a dictionary of credential keys along with an optional credential expiry time. Warning This functionality is considered **unstable** . It may be changed
at any point without it being considered a breaking change.
      - **retries**
      - Number of retries if accessing a cloud instance fails. Deprecated since version 1.37.1: Pass {“max_retries”: n} via `storage_options` instead.
 Examples >>> import pathlib >>> >>> df = pl.DataFrame( ... { ... "foo": [1, 2, 3, 4, 5], ... "bar": [6, 7, 8, 9, 10], ... "ham": ["a", "b", "c", "d", "e"], ... } ... ) >>> path: pathlib.Path = dirpath / "new_file.csv" >>> df.write_csv(path, separator=",")
 [str](https://docs.python.org/3/library/stdtypes.html#str) |[None](https://docs.python.org/3/library/constants.html#None)[\[source\]](https://github.com/pola-rs/polars/blob/py-1.43.2/py-polars/src/../src/polars/dataframe/frame.py#L3066-L3270)  - write_database(
  - *table_name: [str](https://docs.python.org/3/library/stdtypes.html#str)* ,
  - *connection: ConnectionOrCursor | [str](https://docs.python.org/3/library/stdtypes.html#str)* ,
  - *** ,
  - *if_table_exists: DbWriteMode = 'fail'* ,
  - *engine: DbWriteEngine | [None](https://docs.python.org/3/library/constants.html#None) = None* ,
  - *engine_options: [dict](https://docs.python.org/3/library/stdtypes.html#dict)[[str](https://docs.python.org/3/library/stdtypes.html#str), Any] | [None](https://docs.python.org/3/library/constants.html#None) = None* ,
  - Write the data in a Polars DataFrame to a database. Added in version 0.20.26: Support for instantiated connection objects in addition to URI strings, and a new `engine_options` parameter.
    - Parameters:
      - **table_name**
      - Schema-qualified name of the table to create or append to in the target SQL database. If your table name contains special characters, it should be quoted.
      - **connection**
      - An existing SQLAlchemy or ADBC connection against the target database, or a URI string that will be used to instantiate such a connection, such as: 
        - “postgresql://user:pass@server:port/database”
        - “sqlite:////path/to/database.db”
      - **if_table_exists** {‘append’, ‘replace’, ‘fail’}
      - The insert mode: 
        - ‘replace’ will create a new database table, overwriting an existing one.
        - ‘append’ will append to an existing table.
        - ‘fail’ will fail if table already exists.
      - **engine** {‘sqlalchemy’, ‘adbc’}
      - Select the engine to use for writing frame data; only necessary when supplying a URI string (defaults to ‘sqlalchemy’ if unset)
      - **engine_options**
      - Additional options to pass to the insert method associated with the engine specified by the option `engine` .
        - Setting `engine` to “sqlalchemy” currently inserts using Pandas’`to_sql` method (though this will eventually be phased out in favor of a native
solution).
        - Setting `engine` to “adbc” inserts using the ADBC cursor’s`adbc_ingest` method. Note that when passing an instantiated connection object, PyArrow
is required for SQLite and Snowflake drivers.
    - Returns:
      - int
      - The number of rows affected, if the driver provides this information. Otherwise, returns -1.
 Examples Insert into a temporary table using a PostgreSQL URI and the ADBC engine: >>> df.write_database( ... table_name="target_table", ... connection="postgresql://user:pass@server:port/database", ... engine="adbc", ... engine_options={"temporary": True}, ... ) Insert into a table using a `pyodbc` SQLAlchemy connection to SQL Server
that was instantiated with “fast_executemany=True” to improve performance:>>> pyodbc_uri = ( ... "mssql+pyodbc://user:pass@server:1433/test?" ... "driver=ODBC+Driver+18+for+SQL+Server" ... ) >>> engine = create_engine(pyodbc_uri, fast_executemany=True) >>> df.write_database( ... table_name="target_table", ... connection=engine, ... )
 [int](https://docs.python.org/3/library/functions.html#int)[\[source\]](https://github.com/pola-rs/polars/blob/py-1.43.2/py-polars/src/../src/polars/dataframe/frame.py#L4391-L4746)  - write_delta(
  - *target: [str](https://docs.python.org/3/library/stdtypes.html#str) | Path | deltalake.DeltaTable* ,
  - *** ,
  - *mode: Literal['error', 'append', 'overwrite', 'ignore', 'merge'] = 'error'* ,
  - *overwrite_schema: [bool](https://docs.python.org/3/library/functions.html#bool) | [None](https://docs.python.org/3/library/constants.html#None) = None* ,
  - *storage_options: StorageOptionsDict | [None](https://docs.python.org/3/library/constants.html#None) = None* ,
  - *credential_provider: CredentialProviderFunction | Literal['auto'] | [None](https://docs.python.org/3/library/constants.html#None) = 'auto'* ,
  - *delta_write_options: [dict](https://docs.python.org/3/library/stdtypes.html#dict)[[str](https://docs.python.org/3/library/stdtypes.html#str), Any] | [None](https://docs.python.org/3/library/constants.html#None) = None* ,
  - *delta_merge_options: [dict](https://docs.python.org/3/library/stdtypes.html#dict)[[str](https://docs.python.org/3/library/stdtypes.html#str), Any] | [None](https://docs.python.org/3/library/constants.html#None) = None* ,
  - Write DataFrame as delta table. 
    - Parameters:
      - **target**
      - URI of a table or a DeltaTable object.
      - **mode** {‘error’, ‘append’, ‘overwrite’, ‘ignore’, ‘merge’}
      - How to handle existing data. 
        - If ‘error’, throw an error if the table already exists (default).
        - If ‘append’, will add new data.
        - If ‘overwrite’, will replace table with new data.
        - If ‘ignore’, will not write anything if table already exists.
        - If ‘merge’, return a `TableMerger` object to merge data from the DataFrame
with the existing data.
      - **overwrite_schema**
      - If True, allows updating the schema of the table. Deprecated since version 0.20.14: Use the parameter `delta_write_options` instead and pass`{"schema_mode": "overwrite"}` .
      - **storage_options**
      - Extra options for the storage backends supported by `deltalake` .
For cloud storages, this may include configurations for authentication etc.
      - **credential_provider**
      - Provide a function that can be called to provide cloud storage credentials. The function is expected to return a dictionary of credential keys along with an optional credential expiry time. Warning This functionality is considered **unstable** . It may be changed
at any point without it being considered a breaking change.
      - **delta_write_options**
      - Additional keyword arguments while writing a Delta lake Table. See a list of supported write options [here](https://delta-io.github.io/delta-rs/api/delta_writer/#deltalake.write_deltalake) .
      - **delta_merge_options**
      - Keyword arguments which are required to `MERGE` a Delta lake Table.
See a list of supported merge options[here](https://delta-io.github.io/delta-rs/api/delta_table/#deltalake.DeltaTable.merge) .
    - Raises:
      - TypeError
      - If the DataFrame contains unsupported data types.
      - ArrowInvalidError
      - If the DataFrame contains data types that could not be cast to their primitive type.
      - TableNotFoundError
      - If the delta table doesn’t exist and MERGE action is triggered
 Notes The Polars data types `Null` and`Time` are not supported
by the delta protocol specification and will raise a TypeError. Columns
using The`Categorical` data type will be converted to
normal (non-categorical) strings when written.Polars columns are always nullable. To write data to a delta table with non-nullable columns, a custom pyarrow schema has to be passed to the `delta_write_options` . See the last example below.Examples Write a dataframe to the local filesystem as a Delta Lake table. >>> df = pl.DataFrame( ... { ... "foo": [1, 2, 3, 4, 5], ... "bar": [6, 7, 8, 9, 10], ... "ham": ["a", "b", "c", "d", "e"], ... } ... ) >>> table_path = "/path/to/delta-table/" >>> df.write_delta(table_path) Append data to an existing Delta Lake table on the local filesystem. Note that this will fail if the schema of the new data does not match the schema of the existing table. >>> df.write_delta(table_path, mode="append") Overwrite a Delta Lake table as a new version. If the schemas of the new and old data are the same, specifying the `schema_mode` is not required.>>> existing_table_path = "/path/to/delta-table/" >>> df.write_delta( ... existing_table_path, ... mode="overwrite", ... delta_write_options={"schema_mode": "overwrite"}, ... ) Write a DataFrame as a Delta Lake table to a cloud object store like S3. >>> table_path = "s3://bucket/prefix/to/delta-table/" >>> df.write_delta( ... table_path, ... storage_options={ ... "AWS_REGION": "THE_AWS_REGION", ... "AWS_ACCESS_KEY_ID": "THE_AWS_ACCESS_KEY_ID", ... "AWS_SECRET_ACCESS_KEY": "THE_AWS_SECRET_ACCESS_KEY", ... }, ... ) Write DataFrame as a Delta Lake table with non-nullable columns. >>> import pyarrow as pa >>> existing_table_path = "/path/to/delta-table/" >>> df.write_delta( ... existing_table_path, ... delta_write_options={ ... "schema": pa.schema([pa.field("foo", pa.int64(), nullable=False)]) ... }, ... ) Write DataFrame as a Delta Lake table with zstd compression. For all `delta_write_options` keyword arguments, check the deltalake docs[here](https://delta-io.github.io/delta-rs/api/delta_writer/#deltalake.write_deltalake) ,
and for Writer Properties in particular[here](https://delta-io.github.io/delta-rs/api/delta_writer/#deltalake.WriterProperties) .>>> import deltalake >>> df.write_delta( ... table_path, ... delta_write_options={ ... "writer_properties": deltalake.WriterProperties(compression="zstd"), ... }, ... ) Merge the DataFrame with an existing Delta Lake table. For all `TableMerger` methods, check the deltalake docs[here](https://delta-io.github.io/delta-rs/api/delta_table/delta_table_merger/) .>>> df = pl.DataFrame( ... { ... "foo": [1, 2, 3, 4, 5], ... "bar": [6, 7, 8, 9, 10], ... "ham": ["a", "b", "c", "d", "e"], ... } ... ) >>> table_path = "/path/to/delta-table/" >>> ( ... df.write_delta( ... "table_path", ... mode="merge", ... delta_merge_options={ ... "predicate": "s.foo = t.foo", ... "source_alias": "s", ... "target_alias": "t", ... }, ... ) ... .when_matched_update_all() ... .when_not_matched_insert_all() ... .execute() ... )
 [None](https://docs.python.org/3/library/constants.html#None)[\[source\]](https://github.com/pola-rs/polars/blob/py-1.43.2/py-polars/src/../src/polars/dataframe/frame.py#L4811-L5068)  - write_excel(
  - *workbook: [str](https://docs.python.org/3/library/stdtypes.html#str) | Workbook | IO[[bytes](https://docs.python.org/3/library/stdtypes.html#bytes)] | Path | [None](https://docs.python.org/3/library/constants.html#None) = None* ,
  - *worksheet: [str](https://docs.python.org/3/library/stdtypes.html#str) | Worksheet | [None](https://docs.python.org/3/library/constants.html#None) = None* ,
  - *** ,
  - *position: [tuple](https://docs.python.org/3/library/stdtypes.html#tuple)[[int](https://docs.python.org/3/library/functions.html#int), [int](https://docs.python.org/3/library/functions.html#int)] | [str](https://docs.python.org/3/library/stdtypes.html#str) = 'A1'* ,
  - *table_style: [str](https://docs.python.org/3/library/stdtypes.html#str) | [dict](https://docs.python.org/3/library/stdtypes.html#dict)[[str](https://docs.python.org/3/library/stdtypes.html#str), Any] | [None](https://docs.python.org/3/library/constants.html#None) = None* ,
  - *table_name: [str](https://docs.python.org/3/library/stdtypes.html#str) | [None](https://docs.python.org/3/library/constants.html#None) = None* ,
  - *column_formats: ColumnFormatDict | [None](https://docs.python.org/3/library/constants.html#None) = None* ,
  - *dtype_formats: [dict](https://docs.python.org/3/library/stdtypes.html#dict)[OneOrMoreDataTypes, [str](https://docs.python.org/3/library/stdtypes.html#str)] | [None](https://docs.python.org/3/library/constants.html#None) = None* ,
  - *conditional_formats: ConditionalFormatDict | [None](https://docs.python.org/3/library/constants.html#None) = None* ,
  - *header_format: [dict](https://docs.python.org/3/library/stdtypes.html#dict)[[str](https://docs.python.org/3/library/stdtypes.html#str), Any] | [None](https://docs.python.org/3/library/constants.html#None) = None* ,
  - *column_totals: ColumnTotalsDefinition | [None](https://docs.python.org/3/library/constants.html#None) = None* ,
  - *column_widths: ColumnWidthsDefinition | [None](https://docs.python.org/3/library/constants.html#None) = None* ,
  - *row_totals: RowTotalsDefinition | [None](https://docs.python.org/3/library/constants.html#None) = None* ,
  - *row_heights: [dict](https://docs.python.org/3/library/stdtypes.html#dict)[[int](https://docs.python.org/3/library/functions.html#int) | [tuple](https://docs.python.org/3/library/stdtypes.html#tuple)[[int](https://docs.python.org/3/library/functions.html#int), ...], [int](https://docs.python.org/3/library/functions.html#int)] | [int](https://docs.python.org/3/library/functions.html#int) | [None](https://docs.python.org/3/library/constants.html#None) = None* ,
  - *sparklines: [dict](https://docs.python.org/3/library/stdtypes.html#dict)[[str](https://docs.python.org/3/library/stdtypes.html#str), Sequence[[str](https://docs.python.org/3/library/stdtypes.html#str)] | [dict](https://docs.python.org/3/library/stdtypes.html#dict)[[str](https://docs.python.org/3/library/stdtypes.html#str), Any]] | [None](https://docs.python.org/3/library/constants.html#None) = None* ,
  - *formulas: [dict](https://docs.python.org/3/library/stdtypes.html#dict)[[str](https://docs.python.org/3/library/stdtypes.html#str), [str](https://docs.python.org/3/library/stdtypes.html#str) | [dict](https://docs.python.org/3/library/stdtypes.html#dict)[[str](https://docs.python.org/3/library/stdtypes.html#str), [str](https://docs.python.org/3/library/stdtypes.html#str)]] | [None](https://docs.python.org/3/library/constants.html#None) = None* ,
  - *float_precision: [int](https://docs.python.org/3/library/functions.html#int) = 3* ,
  - *include_header: [bool](https://docs.python.org/3/library/functions.html#bool) = True* ,
  - *autofilter: [bool](https://docs.python.org/3/library/functions.html#bool) = True* ,
  - *autofit: [bool](https://docs.python.org/3/library/functions.html#bool) = False* ,
  - *hidden_columns: Sequence[[str](https://docs.python.org/3/library/stdtypes.html#str)] | SelectorType | [None](https://docs.python.org/3/library/constants.html#None) = None* ,
  - *hide_gridlines: [bool](https://docs.python.org/3/library/functions.html#bool) = False* ,
  - *sheet_zoom: [int](https://docs.python.org/3/library/functions.html#int) | [None](https://docs.python.org/3/library/constants.html#None) = None* ,
  - *freeze_panes: [str](https://docs.python.org/3/library/stdtypes.html#str) | [tuple](https://docs.python.org/3/library/stdtypes.html#tuple)[[int](https://docs.python.org/3/library/functions.html#int), [int](https://docs.python.org/3/library/functions.html#int)] | [tuple](https://docs.python.org/3/library/stdtypes.html#tuple)[[str](https://docs.python.org/3/library/stdtypes.html#str), [int](https://docs.python.org/3/library/functions.html#int), [int](https://docs.python.org/3/library/functions.html#int)] | [tuple](https://docs.python.org/3/library/stdtypes.html#tuple)[[int](https://docs.python.org/3/library/functions.html#int), [int](https://docs.python.org/3/library/functions.html#int), [int](https://docs.python.org/3/library/functions.html#int), [int](https://docs.python.org/3/library/functions.html#int)] | [None](https://docs.python.org/3/library/constants.html#None) = None* ,
  - *use_zip64: [bool](https://docs.python.org/3/library/functions.html#bool) = False* ,
  - Write frame data to a table in an Excel workbook/worksheet. 
    - Parameters:
      - **workbook** {str, Workbook}
      - String name or path of the workbook to create, BytesIO object, file opened in binary-mode, or an `xlsxwriter.Workbook` object that has not been closed.
If None, writes to`dataframe.xlsx` in the working directory.
      - **worksheet** {str, Worksheet}
      - Name of target worksheet or an `xlsxwriter.Worksheet` object (in which
case`workbook` must be the parent`xlsxwriter.Workbook` object); if None,
writes to “Sheet1” when creating a new workbook (note that writing to an
existing workbook requires a valid existing -or new- worksheet name).
      - **position** {str, tuple}
      - Table position in Excel notation (eg: “A1”), or a (row,col) integer tuple.
      - **table_style** {str, dict}
      - A named Excel table style, such as “Table Style Medium 4”, or a dictionary of `{"key":value,}` options containing one or more of the following keys:
“style”, “first_column”, “last_column”, “banded_columns, “banded_rows”.
      - **table_name** str
      - Name of the output table object in the worksheet; can then be referred to in the sheet by formulae/charts, or by subsequent `xlsxwriter` operations.
      - **column_formats** dict
      - A `{colname(s):str,}` or`{selector:str,}` dictionary for applying an
Excel format string to the given columns. Formats defined here (such as
“dd/mm/yyyy”, “0.00%”, etc) will override any defined in`dtype_formats` .
      - **dtype_formats** dict
      - A `{dtype:str,}` dictionary that sets the default Excel format for the
given dtype. (This can be overridden on a per-column basis by the`column_formats` param).
      - **conditional_formats** dict
      - A dictionary of colname (or selector) keys to a format str, dict, or list that defines conditional formatting options for the specified columns. 
        - If supplying a string typename, should be one of the valid `xlsxwriter` types such as “3_color_scale”, “data_bar”, etc.
        - If supplying a dictionary you can make use of any/all `xlsxwriter` supported options, including icon sets, formulae, etc.
        - Supplying multiple columns as a tuple/key will apply a single format across all columns - this is effective in creating a heatmap, as the min/max values will be determined across the entire range, not per-column.
        - Finally, you can also supply a list made up from the above options in order to apply *more* than one conditional format to the same range.
      - **header_format** dict
      - A `{key:value,}` dictionary of`xlsxwriter` format options to apply
to the table header row, such as`{"bold":True, "font_color":"#702963"}` .
      - **column_totals** {bool, list, dict}
      - Add a column-total row to the exported table. 
        - If True, all numeric columns will have an associated total using “sum”.
        - If passing a string, it must be one of the valid total function names and all numeric columns will have an associated total using that function.
        - If passing a list of colnames, only those given will have a total.
        - For more control, pass a `{colname:funcname,}` dict.
 Valid column-total function names are “average”, “count_nums”, “count”, “max”, “min”, “std_dev”, “sum”, and “var”.
      - **column_widths** {dict, int}
      - A `{colname:int,}` or`{selector:int,}` dict or a single integer that
sets (or overrides if autofitting) table column widths, in integer pixel
units. If given as an integer the same value is used for all table columns.
      - **row_totals** {dict, list, bool}
      - Add a row-total column to the right-hand side of the exported table. 
        - If True, a column called “total” will be added at the end of the table that applies a “sum” function row-wise across all numeric columns.
        - If passing a list/sequence of column names, only the matching columns will participate in the sum.
        - Can also pass a `{colname:columns,}` dictionary to create one or
more total columns with distinct names, referencing different columns.
      - **row_heights** {dict, int}
      - An int or `{row_index:int,}` dictionary that sets the height of the given
rows (if providing a dictionary) or all rows (if providing an integer) that
intersect with the table body (including any header and total row) in
integer pixel units. Note that`row_index` starts at zero and will be
the header row (unless`include_header` is False).
      - **sparklines** dict
      - A `{colname:list,}` or`{colname:dict,}` dictionary defining one or more
sparklines to be written into a new column in the table.
        - If passing a list of colnames (used as the source of the sparkline data) the default sparkline settings are used (eg: line chart with no markers).
        - For more control an `xlsxwriter` -compliant options dict can be supplied,
in which case three additional polars-specific keys are available:
“columns”, “insert_before”, and “insert_after”. These allow you to define
the source columns and position the sparkline(s) with respect to other
table columns. If no position directive is given, sparklines are added to
the end of the table (eg: to the far right) in the order they are given.
      - **formulas** dict
      - A `{colname:formula,}` or`{colname:dict,}` dictionary defining one or
more formulas to be written into a new column in the table. Note that you
are strongly advised to use structured references in your formulae wherever
possible to make it simple to reference columns by name.
        - If providing a string formula (such as “=[@colx]*[@coly]”) the column will be added to the end of the table (eg: to the far right), after any default sparklines and before any row_totals.
        - For the most control supply an options dictionary with the following keys: “formula” (mandatory), one of “insert_before” or “insert_after”, and optionally “return_dtype”. The latter is used to appropriately format the output of the formula and allow it to participate in row/column totals.
      - **float_precision** int
      - Default number of decimals displayed for floating point columns (note that this is purely a formatting directive; the actual values are not rounded).
      - **include_header** bool
      - Indicate if the table should be created with a header row.
      - **autofilter** bool
      - If the table has headers, provide autofilter capability.
      - **autofit** bool
      - Calculate individual column widths from the data.
      - **hidden_columns** str | list
      - A column name, list of column names, or a selector representing table columns to mark as hidden in the output worksheet.
      - **hide_gridlines** bool
      - Do not display any gridlines on the output worksheet.
      - **sheet_zoom** int
      - Set the default zoom level of the output worksheet.
      - **freeze_panes** str | (str, int, int) | (int, int) | (int, int, int, int)
      - Freeze workbook panes. 
        - If (row, col) is supplied, panes are split at the top-left corner of the specified cell, which are 0-indexed. Thus, to freeze only the top row, supply (1, 0).
        - Alternatively, cell notation can be used to supply the cell. For example, “A2” indicates the split occurs at the top-left of cell A2, which is the equivalent of (1, 0).
        - If (row, col, top_row, top_col) are supplied, the panes are split based on the `row` and`col` , and the scrolling region is initialized to begin at
the`top_row` and`top_col` . Thus, to freeze only the top row and have the
scrolling region begin at row 10, column D (5th col), supply (1, 0, 9, 4).
Using cell notation for (row, col), supplying (“A2”, 9, 4) is equivalent.
      - **use_zip64** bool
      - Whether to use ZIP64 extensions when writing the Workbook. This allows for writing exceptionally large workbook files (>=4GB when uncompressed), but is less broadly compatible.
 Notes 
    - A list of compatible `xlsxwriter` format property names can be found here:[https://xlsxwriter.readthedocs.io/format.html#format-methods-and-format-properties](https://xlsxwriter.readthedocs.io/format.html#format-methods-and-format-properties)
    - Conditional formatting dictionaries should provide xlsxwriter-compatible definitions; polars will take care of how they are applied on the worksheet with respect to the relative sheet/column position. For supported options, see: [https://xlsxwriter.readthedocs.io/working_with_conditional_formats.html](https://xlsxwriter.readthedocs.io/working_with_conditional_formats.html)
    - Similarly, sparkline option dictionaries should contain xlsxwriter-compatible key/values, as well as a mandatory polars “columns” key that defines the sparkline source data; these source columns should all be adjacent. Two other polars-specific keys are available to help define where the sparkline appears in the table: “insert_after”, and “insert_before”. The value associated with these keys should be the name of a column in the exported table. [https://xlsxwriter.readthedocs.io/working_with_sparklines.html](https://xlsxwriter.readthedocs.io/working_with_sparklines.html)
    - Formula dictionaries *must* contain a key called “formula”, and then optional
“insert_after”, “insert_before”, and/or “return_dtype” keys. These additional
keys allow the column to be injected into the table at a specific location,
and/or to define the return type of the formula (eg: “Int64”, “Float64”, etc).
Formulas that refer to table columns should use Excel’s structured references
syntax to ensure the formula is applied correctly and is table-relative.[https://support.microsoft.com/en-us/office/using-structured-references-with-excel-tables-f5ed2452-2337-4f71-bed3-c8ae6d2b276e](https://support.microsoft.com/en-us/office/using-structured-references-with-excel-tables-f5ed2452-2337-4f71-bed3-c8ae6d2b276e)
    - If you want unformatted output, you can use a selector to apply the “General” format to all columns (or all *non-temporal* columns to preserve formatting
of date/datetime columns), eg:`column_formats={~cs.temporal(): "General"}` .
 Examples Instantiate a basic DataFrame: >>> from random import uniform >>> from datetime import date >>> >>> df = pl.DataFrame( ... { ... "dtm": [date(2023, 1, 1), date(2023, 1, 2), date(2023, 1, 3)], ... "num": [uniform(-500, 500), uniform(-500, 500), uniform(-500, 500)], ... "val": [10_000, 20_000, 30_000], ... } ... ) Export to “dataframe.xlsx” (the default workbook name, if not specified) in the working directory, add column totals on all numeric columns (“sum” by default), then autofit: >>> df.write_excel(column_totals=True, autofit=True) Write frame to a specific location on the sheet, set a named table style, apply US-style date formatting, increase floating point formatting precision, apply a non-default column total function to a specific column, autofit: >>> df.write_excel( ... position="B4", ... table_style="Table Style Light 16", ... dtype_formats={pl.Date: "mm/dd/yyyy"}, ... column_totals={"num": "average"}, ... float_precision=6, ... autofit=True, ... ) Write the same frame to a named worksheet twice, applying different styles and conditional formatting to each table, adding custom-formatted table titles using explicit `xlsxwriter` integration:>>> from xlsxwriter import Workbook >>> with Workbook("multi_frame.xlsx") as wb: ... # basic/default conditional formatting ... df.write_excel( ... workbook=wb, ... worksheet="data", ... position=(3, 1), # specify position as (row,col) coordinates ... conditional_formats={"num": "3_color_scale", "val": "data_bar"}, ... table_style="Table Style Medium 4", ... ) ... ... # advanced conditional formatting, custom styles ... df.write_excel( ... workbook=wb, ... worksheet="data", ... position=(df.height + 7, 1), ... table_style={ ... "style": "Table Style Light 4", ... "first_column": True, ... }, ... conditional_formats={ ... "num": { ... "type": "3_color_scale", ... "min_color": "#76933c", ... "mid_color": "#c4d79b", ... "max_color": "#ebf1de", ... }, ... "val": { ... "type": "data_bar", ... "data_bar_2010": True, ... "bar_color": "#9bbb59", ... "bar_negative_color_same": True, ... "bar_negative_border_color_same": True, ... }, ... }, ... column_formats={"num": "#,##0.000;[White]-#,##0.000"}, ... column_widths={"val": 125}, ... autofit=True, ... ) ... ... # add some table titles (with a custom format) ... ws = wb.get_worksheet_by_name("data") ... fmt_title = wb.add_format( ... { ... "font_color": "#4f6228", ... "font_size": 12, ... "italic": True, ... "bold": True, ... } ... ) ... ws.write(2, 1, "Basic/default conditional formatting", fmt_title) ... ws.write(df.height + 6, 1, "Custom conditional formatting", fmt_title) Export a table containing two different types of sparklines. Use default options for the “trend” sparkline and customized options (and positioning) for the “+/-” `win_loss` sparkline, with non-default integer formatting,
column totals, a subtle two-tone heatmap and hidden worksheet gridlines:>>> df = pl.DataFrame( ... { ... "id": ["aaa", "bbb", "ccc", "ddd", "eee"], ... "q1": [100, 55, -20, 0, 35], ... "q2": [30, -10, 15, 60, 20], ... "q3": [-50, 0, 40, 80, 80], ... "q4": [75, 55, 25, -10, -55], ... } ... ) >>> df.write_excel( ... table_style="Table Style Light 2", ... # apply accounting format to all flavours of integer ... dtype_formats={dt: "#,##0_);(#,##0)" for dt in [pl.Int32, pl.Int64]}, ... sparklines={ ... # default options; just provide source cols ... "trend": ["q1", "q2", "q3", "q4"], ... # customized sparkline type, with positioning directive ... "+/-": { ... "columns": ["q1", "q2", "q3", "q4"], ... "insert_after": "id", ... "type": "win_loss", ... }, ... }, ... conditional_formats={ ... # create a unified multi-column heatmap ... ("q1", "q2", "q3", "q4"): { ... "type": "2_color_scale", ... "min_color": "#95b3d7", ... "max_color": "#ffffff", ... }, ... }, ... column_totals=["q1", "q2", "q3", "q4"], ... row_totals=True, ... hide_gridlines=True, ... ) Export a table containing an Excel formula-based column that calculates a standardised Z-score, showing use of structured references in conjunction with positioning directives, column totals, and custom formatting. >>> df = pl.DataFrame( ... { ... "id": ["a123", "b345", "c567", "d789", "e101"], ... "points": [99, 45, 50, 85, 35], ... } ... ) >>> df.write_excel( ... table_style={ ... "style": "Table Style Medium 15", ... "first_column": True, ... }, ... column_formats={ ... "id": {"font": "Consolas"}, ... "points": {"align": "center"}, ... "z-score": {"align": "center"}, ... }, ... column_totals="average", ... formulas={ ... "z-score": { ... # use structured references to refer to the table columns and 'totals' row ... "formula": "=STANDARDIZE([@points], [[#Totals],[points]], STDEV([points]))", ... "insert_after": "points", ... "return_dtype": pl.Float64, ... } ... }, ... hide_gridlines=True, ... sheet_zoom=125, ... ) Create and reference a Worksheet object directly, adding a basic chart. Taking advantage of structured references to set chart series values and categories is *strongly* recommended so you do not have to calculate
cell positions with respect to the frame data and worksheet:>>> with Workbook("basic_chart.xlsx") as wb: ... # create worksheet object and write frame data to it ... ws = wb.add_worksheet("demo") ... df.write_excel( ... workbook=wb, ... worksheet=ws, ... table_name="DataTable", ... table_style="Table Style Medium 26", ... hide_gridlines=True, ... ) ... # create chart object, point to the written table ... # data using structured references, and style it ... chart = wb.add_chart({"type": "column"}) ... chart.set_title({"name": "Example Chart"}) ... chart.set_legend({"none": True}) ... chart.set_style(38) ... chart.add_series( ... { # note the use of structured references ... "values": "=DataTable[points]", ... "categories": "=DataTable[id]", ... "data_labels": {"value": True}, ... } ... ) ... # add chart to the worksheet ... ws.insert_chart("D1", chart) Export almost entirely unformatted data (no numeric styling or standardised floating point precision), omit autofilter, but keep date/datetime formatting: >>> import polars.selectors as cs >>> df = pl.DataFrame( ... { ... "n1": [-100, None, 200, 555], ... "n2": [987.4321, -200, 44.444, 555.5], ... } ... ) >>> df.write_excel( ... column_formats={~cs.temporal(): "General"}, ... autofilter=False, ... )
 [\[source\]](https://github.com/pola-rs/polars/blob/py-1.43.2/py-polars/src/../src/polars/dataframe/frame.py#L3334-L3914)  - write_iceberg(
  - *target: [str](https://docs.python.org/3/library/stdtypes.html#str) | pyiceberg.table.Table* ,
  - *mode: Literal['append', 'overwrite']* ,
  - Write DataFrame to an Iceberg table. Warning This functionality is currently considered **unstable** . It may be
changed at any point without it being considered a breaking change.
    - Parameters:
      - **target**
      - Name of the table or the Table object representing an Iceberg table.
      - **mode** {‘append’, ‘overwrite’}
      - How to handle existing data. 
        - If ‘append’, will add new data.
        - If ‘overwrite’, will replace table with new data.
 [None](https://docs.python.org/3/library/constants.html#None)[\[source\]](https://github.com/pola-rs/polars/blob/py-1.43.2/py-polars/src/../src/polars/dataframe/frame.py#L4748-L4785)  - write_ipc(
  - *file: [str](https://docs.python.org/3/library/stdtypes.html#str) | Path | IO[[bytes](https://docs.python.org/3/library/stdtypes.html#bytes)] | [None](https://docs.python.org/3/library/constants.html#None)* ,
  - *** ,
  - *compression: IpcCompression = 'uncompressed'* ,
  - *compat_level: CompatLevel | [None](https://docs.python.org/3/library/constants.html#None) = None* ,
  - *record_batch_size: [int](https://docs.python.org/3/library/functions.html#int) | [None](https://docs.python.org/3/library/constants.html#None) = None* ,
  - *storage_options: StorageOptionsDict | [None](https://docs.python.org/3/library/constants.html#None) = None* ,
  - *credential_provider: CredentialProviderFunction | Literal['auto'] | [None](https://docs.python.org/3/library/constants.html#None) = 'auto'* ,
  - *retries: [int](https://docs.python.org/3/library/functions.html#int) | [None](https://docs.python.org/3/library/constants.html#None) = None* ,
  - Write to Arrow IPC binary stream or Feather file. See “File or Random Access format” in [https://arrow.apache.org/docs/python/ipc.html](https://arrow.apache.org/docs/python/ipc.html) .Changed in version 1.1: The `future` parameter was renamed`compat_level` .
    - Parameters:
      - **file**
      - Path or writable file-like object to which the IPC data will be written. If set to `None` , the output is returned as a BytesIO object.
      - **compression** {‘uncompressed’, ‘lz4’, ‘zstd’}
      - Compression method. Defaults to “uncompressed”.
      - **compat_level**
      - Use a specific compatibility level when exporting Polars’ internal data structures.
      - **record_batch_size**
      - Size of the record batches in number of rows. Warning This functionality is considered **unstable** . It may be changed
at any point without it being considered a breaking change.
      - **storage_options**
      - Options that indicate how to connect to a cloud provider. The cloud providers currently supported are AWS, GCP, and Azure. See supported keys here: 
        - [aws](https://docs.rs/object_store/latest/object_store/aws/enum.AmazonS3ConfigKey.html)
        - [gcp](https://docs.rs/object_store/latest/object_store/gcp/enum.GoogleConfigKey.html)
        - [azure](https://docs.rs/object_store/latest/object_store/azure/enum.AzureConfigKey.html)
        - Hugging Face ( `hf://` ): Accepts an API key under the`token` parameter:`{'token': '...'}` , or by setting the`HF_TOKEN` environment variable.
 If `storage_options` is not provided, Polars will try to infer the
information from environment variables.
      - **credential_provider**
      - Provide a function that can be called to provide cloud storage credentials. The function is expected to return a dictionary of credential keys along with an optional credential expiry time. Warning This functionality is considered **unstable** . It may be changed
at any point without it being considered a breaking change.
      - **retries**
      - Number of retries if accessing a cloud instance fails. Deprecated since version 1.37.1: Pass {“max_retries”: n} via `storage_options` instead.
 Examples >>> import pathlib >>> >>> df = pl.DataFrame( ... { ... "foo": [1, 2, 3, 4, 5], ... "bar": [6, 7, 8, 9, 10], ... "ham": ["a", "b", "c", "d", "e"], ... } ... ) >>> path: pathlib.Path = dirpath / "new_file.arrow" >>> df.write_ipc(path) Write to a `BytesIO` object by passing`file=None` . The returned
buffer’s position is at the end of the written data, so call`seek(0)` before reading it back.>>> buf = df.write_ipc(file=None) >>> buf.seek(0) 0 >>> pl.read_ipc(buf).equals(df) True
 [None](https://docs.python.org/3/library/constants.html#None)[\[source\]](https://github.com/pola-rs/polars/blob/py-1.43.2/py-polars/src/../src/polars/dataframe/frame.py#L3946-L4057)  - write_ipc_stream(
  - *file: [str](https://docs.python.org/3/library/stdtypes.html#str) | Path | IO[[bytes](https://docs.python.org/3/library/stdtypes.html#bytes)] | [None](https://docs.python.org/3/library/constants.html#None)* ,
  - *** ,
  - *compression: IpcCompression = 'uncompressed'* ,
  - *compat_level: CompatLevel | [None](https://docs.python.org/3/library/constants.html#None) = None* ,
  - Write to Arrow IPC record batch stream. See “Streaming format” in [https://arrow.apache.org/docs/python/ipc.html](https://arrow.apache.org/docs/python/ipc.html) .Changed in version 1.1: The `future` parameter was renamed`compat_level` .
    - Parameters:
      - **file**
      - Path or writable file-like object to which the IPC record batch data will be written. If set to `None` , the output is returned as a BytesIO object.
      - **compression** {‘uncompressed’, ‘lz4’, ‘zstd’}
      - Compression method. Defaults to “uncompressed”.
      - **compat_level**
      - Use a specific compatibility level when exporting Polars’ internal data structures.
 Examples >>> import pathlib >>> >>> df = pl.DataFrame( ... { ... "foo": [1, 2, 3, 4, 5], ... "bar": [6, 7, 8, 9, 10], ... "ham": ["a", "b", "c", "d", "e"], ... } ... ) >>> path: pathlib.Path = dirpath / "new_file.arrow" >>> df.write_ipc_stream(path)
 [None](https://docs.python.org/3/library/constants.html#None)[\[source\]](https://github.com/pola-rs/polars/blob/py-1.43.2/py-polars/src/../src/polars/dataframe/frame.py#L4077-L4134)  - 
write_json(*file: IOBase | [str](https://docs.python.org/3/library/stdtypes.html#str) | Path | [None](https://docs.python.org/3/library/constants.html#None) = None* )[str](https://docs.python.org/3/library/stdtypes.html#str) |[None](https://docs.python.org/3/library/constants.html#None)[\[source\]](https://github.com/pola-rs/polars/blob/py-1.43.2/py-polars/src/../src/polars/dataframe/frame.py#L2867-L2911)
  - Serialize to JSON representation. 
    - Parameters:
      - **file**
      - File path or writable file-like object to which the result will be written. If set to `None` (default), the output is returned as a string instead.
 See also Examples >>> df = pl.DataFrame( ... { ... "foo": [1, 2, 3], ... "bar": [6, 7, 8], ... } ... ) >>> df.write_json() '[{"foo":1,"bar":6},{"foo":2,"bar":7},{"foo":3,"bar":8}]'
 
  - write_ndjson(
  - *file: [str](https://docs.python.org/3/library/stdtypes.html#str) | Path | IO[[bytes](https://docs.python.org/3/library/stdtypes.html#bytes)] | IO[[str](https://docs.python.org/3/library/stdtypes.html#str)] | [None](https://docs.python.org/3/library/constants.html#None) = None* ,
  - *** ,
  - *compression: Literal['uncompressed', 'gzip', 'zstd'] = 'uncompressed'* ,
  - *compression_level: [int](https://docs.python.org/3/library/functions.html#int) | [None](https://docs.python.org/3/library/constants.html#None) = None* ,
  - *check_extension: [bool](https://docs.python.org/3/library/functions.html#bool) = True* ,
  - Serialize to newline delimited JSON representation. 
    - Parameters:
      - **file**
      - File path or writable file-like object to which the result will be written. If set to `None` (default), the output is returned as a string instead.
      - **compression**
      - What compression format to use. Warning This functionality is considered **unstable** . It may be changed at any
point without it being considered a breaking change.
      - **compression_level**
      - The compression level to use, typically 0-9 or `None` to let the
engine choose.Warning This functionality is considered **unstable** . It may be changed at any
point without it being considered a breaking change.
      - **check_extension**
      - Whether to check if the filename matches the compression settings. Will raise an error if compression is set to ‘uncompressed’ and the filename ends in one of (“.gz”, “.zst”, “.zstd”) or if compression != ‘uncompressed’ and the file uses an mismatched extension. Only applies if file is a path. Warning This functionality is considered **unstable** . It may be changed at any
point without it being considered a breaking change.
 Examples >>> df = pl.DataFrame( ... { ... "foo": [1, 2, 3], ... "bar": [6, 7, 8], ... } ... ) >>> df.write_ndjson() '{"foo":1,"bar":6}\n{"foo":2,"bar":7}\n{"foo":3,"bar":8}\n'
 [str](https://docs.python.org/3/library/stdtypes.html#str) |[None](https://docs.python.org/3/library/constants.html#None)[\[source\]](https://github.com/pola-rs/polars/blob/py-1.43.2/py-polars/src/../src/polars/dataframe/frame.py#L2933-L3010)  - write_parquet(
  - *file: [str](https://docs.python.org/3/library/stdtypes.html#str) | Path | IO[[bytes](https://docs.python.org/3/library/stdtypes.html#bytes)]* ,
  - *** ,
  - *compression: ParquetCompression = 'zstd'* ,
  - *compression_level: [int](https://docs.python.org/3/library/functions.html#int) | [None](https://docs.python.org/3/library/constants.html#None) = None* ,
  - *statistics: [bool](https://docs.python.org/3/library/functions.html#bool) | [str](https://docs.python.org/3/library/stdtypes.html#str) | [dict](https://docs.python.org/3/library/stdtypes.html#dict)[[str](https://docs.python.org/3/library/stdtypes.html#str), [bool](https://docs.python.org/3/library/functions.html#bool)] = True* ,
  - *row_group_size: [int](https://docs.python.org/3/library/functions.html#int) | [None](https://docs.python.org/3/library/constants.html#None) = None* ,
  - *data_page_size: [int](https://docs.python.org/3/library/functions.html#int) | [None](https://docs.python.org/3/library/constants.html#None) = None* ,
  - *use_pyarrow: [bool](https://docs.python.org/3/library/functions.html#bool) = False* ,
  - *pyarrow_options: [dict](https://docs.python.org/3/library/stdtypes.html#dict)[[str](https://docs.python.org/3/library/stdtypes.html#str), Any] | [None](https://docs.python.org/3/library/constants.html#None) = None* ,
  - *partition_by: [str](https://docs.python.org/3/library/stdtypes.html#str) | Sequence[[str](https://docs.python.org/3/library/stdtypes.html#str)] | [None](https://docs.python.org/3/library/constants.html#None) = None* ,
  - *partition_chunk_size_bytes: [int](https://docs.python.org/3/library/functions.html#int) = 4294967296* ,
  - *storage_options: StorageOptionsDict | [None](https://docs.python.org/3/library/constants.html#None) = None* ,
  - *credential_provider: CredentialProviderFunction | Literal['auto'] | [None](https://docs.python.org/3/library/constants.html#None) = 'auto'* ,
  - *retries: [int](https://docs.python.org/3/library/functions.html#int) | [None](https://docs.python.org/3/library/constants.html#None) = None* ,
  - *metadata: ParquetMetadata | [None](https://docs.python.org/3/library/constants.html#None) = None* ,
  - *arrow_schema: ArrowSchemaExportable | [None](https://docs.python.org/3/library/constants.html#None) = None* ,
  - *mkdir: [bool](https://docs.python.org/3/library/functions.html#bool) = False* ,
  - Write to Apache Parquet file. 
    - Parameters:
      - **file**
      - File path or writable file-like object to which the result will be written. This should be a path to a directory if writing a partitioned dataset.
      - **compression** {‘lz4’, ‘uncompressed’, ‘snappy’, ‘gzip’, ‘brotli’, ‘zstd’}
      - Choose “zstd” for good compression performance. Choose “lz4” for fast compression/decompression. Choose “snappy” for more backwards compatibility guarantees when you deal with older parquet readers.
      - **compression_level**
      - The level of compression to use. Higher compression means smaller files on disk. 
        - “gzip” : min-level: 0, max-level: 9, default: 6.
        - “brotli” : min-level: 0, max-level: 11, default: 1.
        - “zstd” : min-level: 1, max-level: 22, default: 3.
      - **statistics**
      - Write statistics to the parquet headers. This is the default behavior. Possible values: 
        - `True` : enable default set of statistics (default). Some
statistics may be disabled.
        - `False` : disable all statistics
        - “full”: calculate and write all available statistics. Cannot be combined with `use_pyarrow` .
        - `{ "statistic-key": True / False, ... }` . Cannot be combined with`use_pyarrow` . Available keys:
          - “min”: column minimum value (default: `True` )
          - “max”: column maximum value (default: `True` )
          - “distinct_count”: number of unique column values (default: `False` )
          - “null_count”: number of null values in column (default: `True` )
      - **row_group_size**
      - Size of the row groups in number of rows. Defaults to 512^2 rows.
      - **data_page_size**
      - Size of the data page in bytes. Defaults to 1024^2 bytes.
      - **use_pyarrow**
      - Use PyArrow’s C++ parquet implementation instead of Polars’ native Rust implementation. This may be useful when specific PyArrow features are needed via `pyarrow_options` . Some options are not supported when
enabled (e.g.`statistics="full"` ,`metadata` ,`mkdir` ).
      - **pyarrow_options**
      - Arguments passed to `pyarrow.parquet.write_table` .If you pass `partition_cols` here, the dataset will be written
using`pyarrow.parquet.write_to_dataset` .
The`partition_cols` parameter leads to write the dataset to a directory.
Similar to Spark’s partitioned datasets. For native partitioned
writes, consider using`partition_by` instead.
      - **partition_by**
      - Column(s) to partition by. A partitioned dataset will be written if this is specified. This parameter is considered unstable and is subject to change.
      - **partition_chunk_size_bytes**
      - Approximate size to split DataFrames within a single partition when writing. Note this is calculated using the size of the DataFrame in memory - the size of the output file may differ depending on the file format / compression.
      - **storage_options**
      - Options that indicate how to connect to a cloud provider. The cloud providers currently supported are AWS, GCP, and Azure. See supported keys here: 
        - [aws](https://docs.rs/object_store/latest/object_store/aws/enum.AmazonS3ConfigKey.html)
        - [gcp](https://docs.rs/object_store/latest/object_store/gcp/enum.GoogleConfigKey.html)
        - [azure](https://docs.rs/object_store/latest/object_store/azure/enum.AzureConfigKey.html)
        - Hugging Face ( `hf://` ): Accepts an API key under the`token` parameter:`{'token': '...'}` , or by setting the`HF_TOKEN` environment variable.
 If `storage_options` is not provided, Polars will try to infer the
information from environment variables.
      - **credential_provider**
      - Provide a function that can be called to provide cloud storage credentials. The function is expected to return a dictionary of credential keys along with an optional credential expiry time. Warning This functionality is considered **unstable** . It may be changed
at any point without it being considered a breaking change.
      - **retries**
      - Number of retries if accessing a cloud instance fails. Deprecated since version 1.37.1: Pass {“max_retries”: n} via `storage_options` instead.
      - **metadata**
      - A dictionary or callback to add key-values to the file-level Parquet metadata. Warning This functionality is considered **experimental** . It may be removed or
changed at any point without it being considered a breaking change.
      - **arrow_schema**
      - Provide a custom arrow schema to write to the file. This allows setting custom schema and field-level metadata. Names and dtypes must match. Warning This functionality is considered **unstable** . It may be changed at any
point without it being considered a breaking change.
      - **mkdir: bool**
      - Recursively create all the directories in the path. Warning This functionality is considered **unstable** . It may be changed at any
point without it being considered a breaking change.
 Examples >>> import pathlib >>> >>> df = pl.DataFrame( ... { ... "foo": [1, 2, 3, 4, 5], ... "bar": [6, 7, 8, 9, 10], ... "ham": ["a", "b", "c", "d", "e"], ... } ... ) >>> path: pathlib.Path = dirpath / "new_file.parquet" >>> df.write_parquet(path) We can write partitioned datasets. The following example will write the first row to ../watermark=1/ *.parquet and the other rows to
../watermark=2/* .parquet.>>> df = pl.DataFrame({"a": [1, 2, 3], "watermark": [1, 2, 2]}) >>> path: pathlib.Path = dirpath / "partitioned_object" >>> df.write_parquet( ... path, ... partition_by=["watermark"], ... )
 [None](https://docs.python.org/3/library/constants.html#None)[\[source\]](https://github.com/pola-rs/polars/blob/py-1.43.2/py-polars/src/../src/polars/dataframe/frame.py#L4136-L4389)

[\[source\]](https://github.com/pola-rs/polars/blob/py-1.43.2/py-polars/src/../src/polars/dataframe/frame.py#L211-L13461)

# Citations

1. Source page: https://docs.pola.rs/api/python/stable/reference/dataframe/index.html
