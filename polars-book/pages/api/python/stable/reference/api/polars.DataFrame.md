---
type: Web Page
title: polars.DataFrame.write_csv — Polars  documentation
resource: https://docs.pola.rs/api/python/stable/reference/api/polars.DataFrame.write_csv.html
timestamp: '2026-08-24T07:00:57.822031+00:00'
---

# polars.DataFrame.write_csv

- DataFrame.write_csv(
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

[str](https://docs.python.org/3/library/stdtypes.html#str)|

[None](https://docs.python.org/3/library/constants.html#None)

[\[source\]](https://github.com/pola-rs/polars/blob/py-1.43.2/py-polars/src/../src/polars/dataframe/frame.py#L3066-L3270)

# Citations

1. Source page: https://docs.pola.rs/api/python/stable/reference/api/polars.DataFrame.write_csv.html
