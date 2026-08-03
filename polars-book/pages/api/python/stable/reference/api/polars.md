---
type: Web Page
title: polars.scan_csv — Polars  documentation
resource: https://docs.pola.rs/api/python/stable/reference/api/polars.scan_csv.html
timestamp: '2026-08-03T09:49:29.273788+00:00'
---

# polars.scan_csv

- polars.scan_csv(
- *source: [str](https://docs.python.org/3/library/stdtypes.html#str) | Path | IO[[str](https://docs.python.org/3/library/stdtypes.html#str)] | IO[[bytes](https://docs.python.org/3/library/stdtypes.html#bytes)] | [bytes](https://docs.python.org/3/library/stdtypes.html#bytes) | [list](https://docs.python.org/3/library/stdtypes.html#list)[[str](https://docs.python.org/3/library/stdtypes.html#str)] | [list](https://docs.python.org/3/library/stdtypes.html#list)[Path] | [list](https://docs.python.org/3/library/stdtypes.html#list)[IO[[str](https://docs.python.org/3/library/stdtypes.html#str)]] | [list](https://docs.python.org/3/library/stdtypes.html#list)[IO[[bytes](https://docs.python.org/3/library/stdtypes.html#bytes)]] | [list](https://docs.python.org/3/library/stdtypes.html#list)[[bytes](https://docs.python.org/3/library/stdtypes.html#bytes)]* ,
- *** ,
- *has_header: [bool](https://docs.python.org/3/library/functions.html#bool) = True* ,
- *separator: [str](https://docs.python.org/3/library/stdtypes.html#str) = ','* ,
- *comment_prefix: [str](https://docs.python.org/3/library/stdtypes.html#str) | [None](https://docs.python.org/3/library/constants.html#None) = None* ,
- *quote_char: [str](https://docs.python.org/3/library/stdtypes.html#str) | [None](https://docs.python.org/3/library/constants.html#None) = '"'* ,
- *skip_rows: [int](https://docs.python.org/3/library/functions.html#int) = 0* ,
- *skip_lines: [int](https://docs.python.org/3/library/functions.html#int) = 0* ,
- *schema: SchemaDict | [None](https://docs.python.org/3/library/constants.html#None) = None* ,
- *schema_overrides: SchemaDict | Sequence[PolarsDataType] | [None](https://docs.python.org/3/library/constants.html#None) = None* ,
- *null_values: [str](https://docs.python.org/3/library/stdtypes.html#str) | Sequence[[str](https://docs.python.org/3/library/stdtypes.html#str)] | [dict](https://docs.python.org/3/library/stdtypes.html#dict)[[str](https://docs.python.org/3/library/stdtypes.html#str), [str](https://docs.python.org/3/library/stdtypes.html#str)] | [None](https://docs.python.org/3/library/constants.html#None) = None* ,
- *empty_string_is_null: [bool](https://docs.python.org/3/library/functions.html#bool) = True* ,
- *ignore_errors: [bool](https://docs.python.org/3/library/functions.html#bool) = False* ,
- *cache: [bool](https://docs.python.org/3/library/functions.html#bool) | [None](https://docs.python.org/3/library/constants.html#None) = None* ,
- *with_column_names: Callable[[[list](https://docs.python.org/3/library/stdtypes.html#list)[[str](https://docs.python.org/3/library/stdtypes.html#str)]], [list](https://docs.python.org/3/library/stdtypes.html#list)[[str](https://docs.python.org/3/library/stdtypes.html#str)]] | [None](https://docs.python.org/3/library/constants.html#None) = None* ,
- *infer_schema: [bool](https://docs.python.org/3/library/functions.html#bool) = True* ,
- *infer_schema_length: [int](https://docs.python.org/3/library/functions.html#int) | [None](https://docs.python.org/3/library/constants.html#None) = 100* ,
- *infer_schema_files: [int](https://docs.python.org/3/library/functions.html#int) = 18446744073709551615* ,
- *n_rows: [int](https://docs.python.org/3/library/functions.html#int) | [None](https://docs.python.org/3/library/constants.html#None) = None* ,
- *encoding: CsvEncoding = 'utf8'* ,
- *low_memory: [bool](https://docs.python.org/3/library/functions.html#bool) = False* ,
- *rechunk: [bool](https://docs.python.org/3/library/functions.html#bool) = False* ,
- *skip_rows_after_header: [int](https://docs.python.org/3/library/functions.html#int) = 0* ,
- *row_index_name: [str](https://docs.python.org/3/library/stdtypes.html#str) | [None](https://docs.python.org/3/library/constants.html#None) = None* ,
- *row_index_offset: [int](https://docs.python.org/3/library/functions.html#int) = 0* ,
- *try_parse_dates: [bool](https://docs.python.org/3/library/functions.html#bool) = False* ,
- *eol_char: [str](https://docs.python.org/3/library/stdtypes.html#str) = '\n'* ,
- *new_columns: Sequence[[str](https://docs.python.org/3/library/stdtypes.html#str)] | [None](https://docs.python.org/3/library/constants.html#None) = None* ,
- *raise_if_empty: [bool](https://docs.python.org/3/library/functions.html#bool) = True* ,
- *truncate_ragged_lines: [bool](https://docs.python.org/3/library/functions.html#bool) = False* ,
- *decimal_comma: [bool](https://docs.python.org/3/library/functions.html#bool) = False* ,
- *glob: [bool](https://docs.python.org/3/library/functions.html#bool) = True* ,
- *storage_options: StorageOptionsDict | [None](https://docs.python.org/3/library/constants.html#None) = None* ,
- *credential_provider: CredentialProviderFunction | Literal['auto'] | [None](https://docs.python.org/3/library/constants.html#None) = 'auto'* ,
- *retries: [int](https://docs.python.org/3/library/functions.html#int) | [None](https://docs.python.org/3/library/constants.html#None) = None* ,
- *file_cache_ttl: [int](https://docs.python.org/3/library/functions.html#int) | [None](https://docs.python.org/3/library/constants.html#None) = None* ,
- *include_file_paths: [str](https://docs.python.org/3/library/stdtypes.html#str) | [None](https://docs.python.org/3/library/constants.html#None) = None* ,
- *missing_columns: Literal['insert', 'raise'] | [None](https://docs.python.org/3/library/constants.html#None) = None* ,
- Lazily read from a CSV file or multiple files via glob patterns. This allows the query optimizer to push down predicates and projections to the scan level, thereby potentially reducing memory overhead. Changed in version 0.20.31: The `dtypes` parameter was renamed`schema_overrides` .Changed in version 0.20.4: * The `row_count_name` parameter was renamed`row_index_name` .
* The`row_count_offset` parameter was renamed`row_index_offset` .
  - Parameters:
    - **source**
    - Path(s) to a file or directory When needing to authenticate for scanning cloud locations, see the `storage_options` parameter.
    - **has_header**
    - Indicate if the first row of the dataset is a header or not. If set to False, column names will be autogenerated in the following format: `column_x` , with`x` being an enumeration over every column in the dataset, starting at 1.
    - **separator**
    - Single byte character to use as separator in the file.
    - **comment_prefix**
    - A string used to indicate the start of a comment line. Comment lines are skipped during parsing. Common examples of comment prefixes are `#` and`//` .
    - **quote_char**
    - Single byte character used for csv quoting, default = `"` .
Set to None to turn off special handling and escaping of quotes.
    - **skip_rows**
    - Start reading after `skip_rows` rows. The header will be parsed at this
offset. Note that we respect CSV escaping/comments when skipping rows.
If you want to skip by newline char only, use`skip_lines` .
    - **skip_lines**
    - Start reading after `skip_lines` lines. The header will be parsed at this
offset. Note that CSV escaping will not be respected when skipping lines.
If you want to skip valid CSV rows, use`skip_rows` .
    - **schema**
    - Provide the schema. This means that polars doesn’t do schema inference. This argument expects the complete schema, whereas `schema_overrides` can be
used to partially overwrite a schema. Note that the order of the columns in
the provided`schema` must match the order of the columns in the CSV being read.
    - **schema_overrides**
    - Overwrite dtypes during inference; should be a {colname:dtype,} dict or, if providing a list of strings to `new_columns` , a list of dtypes of
the same length.
    - **null_values**
    - Values to interpret as null values. You can provide a: 
      - `str` : All values equal to this string will be null.
      - `List[str]` : All values equal to any string in this list will be null.
      - `Dict[str, str]` : A dictionary that maps column name to a
null value string.
    - **empty_string_is_null**
    - By default a missing string value is considered to be null. If `empty_string_is_null` is set to False, missing string values are considered to
decoded as empty strings.
    - **ignore_errors**
    - Try to keep reading lines if some lines yield errors. First try `infer_schema=False` to read all columns as`pl.String` to check which values might cause an issue.
    - **cache**
    - Cache the result after reading. Deprecated since version 1.39.0: File cache is no longer supported.
    - **with_column_names**
    - Apply a function over the column names just in time (when they are determined); this function will receive (and should return) a list of column names.
    - **infer_schema**
    - When `True` , the schema is inferred from the data using the first`infer_schema_length` rows.
When`False` , the schema is not inferred and will be`pl.String` if not
specified in`schema` or`schema_overrides` .
    - **infer_schema_length**
    - The maximum number of rows to scan for schema inference. This applies individually to each file included according to `infer_schema_files` .
If set to`None` , the full data will be scanned into memory**(this is slow)** .
Alternatively set`infer_schema=False` to read all columns as`pl.String` .
    - **infer_schema_files**
    - How many files to use when inferring schema. Warning This functionality is considered **unstable** . It may be changed
at any point without it being considered a breaking change.
    - **n_rows**
    - Stop reading from CSV file after reading `n_rows` .
    - **encoding** {‘utf8’, ‘utf8-lossy’}
    - Lossy means that invalid utf8 values are replaced with `�` characters. Defaults to “utf8”.
    - **low_memory**
    - Reduce memory pressure at the expense of performance.
    - **rechunk**
    - Reallocate to contiguous memory when all chunks/ files are parsed.
    - **skip_rows_after_header**
    - Skip this number of rows when the header is parsed.
    - **row_index_name**
    - If not None, this will insert a row index column with the given name into the DataFrame.
    - **row_index_offset**
    - Offset to start the row index column (only used if the name is set).
    - **try_parse_dates**
    - Try to automatically parse dates. Most ISO8601-like formats can be inferred, as well as a handful of others. If this does not succeed, the column remains of data type `pl.String` .
    - **eol_char**
    - Single byte end of line character (default: `\n` ). When encountering a file
with windows line endings (`\r\n` ), one can go with the default`\n` . The
extra`\r` will be removed when processed.
    - **new_columns**
    - Provide an explicit list of string column names to use (for example, when scanning a headerless CSV file). If the given list is shorter than the width of the DataFrame the remaining columns will have their original name.
    - **raise_if_empty**
    - When there is no data in the source, `NoDataError` is raised. If this parameter
is set to False, an empty LazyFrame (with no columns) is returned instead.
    - **truncate_ragged_lines**
    - Truncate lines that are longer than the schema.
    - **decimal_comma**
    - Parse floats using a comma as the decimal separator instead of a period.
    - **glob**
    - Expand path given via globbing rules.
    - **storage_options**
    - Options that indicate how to connect to a cloud provider. The cloud providers currently supported are AWS, GCP, and Azure. See supported keys here: 
      - [aws](https://docs.rs/object_store/latest/object_store/aws/enum.AmazonS3ConfigKey.html)
      - [gcp](https://docs.rs/object_store/latest/object_store/gcp/enum.GoogleConfigKey.html)
      - [azure](https://docs.rs/object_store/latest/object_store/azure/enum.AzureConfigKey.html)
      - Hugging Face ( `hf://` ): Accepts an API key under the`token` parameter:`{'token': '...'}` , or by setting the`HF_TOKEN` environment variable.
 If `storage_options` is not provided, Polars will try to infer the information
from environment variables.
    - **credential_provider**
    - Provide a function that can be called to provide cloud storage credentials. The function is expected to return a dictionary of credential keys along with an optional credential expiry time. Warning This functionality is considered **unstable** . It may be changed
at any point without it being considered a breaking change.
    - **retries**
    - Number of retries if accessing a cloud instance fails. Deprecated since version 1.37.1: Pass {“max_retries”: n} via `storage_options` instead.
    - **file_cache_ttl**
    - Amount of time to keep downloaded cloud files since their last access time, in seconds. Uses the `POLARS_FILE_CACHE_TTL` environment variable
(which defaults to 1 hour) if not given.Deprecated since version 1.39.0: File cache is no longer supported.
    - **include_file_paths**
    - Include the path of the source file(s) as a column with this name.
    - **missing_columns**
    - Configuration for behavior when columns defined in the schema are missing from the data: 
      - `"insert"` : Insert the missing columns with NULL values.
      - `"raise"` : Raise an error.
 Warning This functionality is considered **unstable** . It may be changed
at any point without it being considered a breaking change.
  - Returns:
    - LazyFrame
 See also 
  - [`read_csv`](polars.read_csv.html#polars.read_csv)
  - Read a CSV file into a DataFrame.
 Examples >>> import pathlib >>> >>> ( ... pl.scan_csv("my_long_file.csv") # lazy, doesn't do a thing ... .select( ... ["a", "c"] ... ) # select only 2 columns (other columns will not be read) ... .filter( ... pl.col("a") > 10 ... ) # the filter is pushed down the scan, so less data is read into memory ... .head(100) # constrain number of returned results to 100 ... ) We can use `with_column_names` to modify the header before scanning:>>> df = pl.DataFrame( ... {"BrEeZaH": [1, 2, 3, 4], "LaNgUaGe": ["is", "hard", "to", "read"]} ... ) >>> path: pathlib.Path = dirpath / "mydf.csv" >>> df.write_csv(path) >>> pl.scan_csv( ... path, with_column_names=lambda cols: [col.lower() for col in cols] ... ).collect() shape: (4, 2) ┌─────────┬──────────┐ │ breezah ┆ language │ │ --- ┆ --- │ │ i64 ┆ str │ ╞═════════╪══════════╡ │ 1 ┆ is │ │ 2 ┆ hard │ │ 3 ┆ to │ │ 4 ┆ read │ └─────────┴──────────┘ You can also simply replace column names (or provide them if the file has none) by passing a list of new column names to the `new_columns` parameter:>>> df.write_csv(path) >>> pl.scan_csv( ... path, ... new_columns=["idx", "txt"], ... schema_overrides=[pl.UInt16, pl.String], ... ).collect() shape: (4, 2) ┌─────┬──────┐ │ idx ┆ txt │ │ --- ┆ --- │ │ u16 ┆ str │ ╞═════╪══════╡ │ 1 ┆ is │ │ 2 ┆ hard │ │ 3 ┆ to │ │ 4 ┆ read │ └─────┴──────┘

[\[source\]](https://github.com/pola-rs/polars/blob/py-1.43.2/py-polars/src/../src/polars/io/csv/functions.py#L1098-L1479)

# Citations

1. Source page: https://docs.pola.rs/api/python/stable/reference/api/polars.scan_csv.html
