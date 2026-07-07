---
type: Web Page
title: Google Sheets (via Colab) - Polars user guide
resource: https://docs.pola.rs/user-guide/io/sheets_colab
timestamp: '2026-07-07T12:26:19.464100+00:00'
---

# Google Sheets (via Colab)

Google Colab provides a utility class to read from and write to Google Sheets.

## Opening and reading from a sheet

We can open existing sheets by initializing `sheets.InteractiveSheet` with either:

- the `url`parameter, for example https://docs.google.com/spreadsheets/d/1BxiMVs0XRA5nFMdKvBdBZjgmUUqptlbs74OgvE2upms/
- the `sheet_id`parameter for example 1BxiMVs0XRA5nFMdKvBdBZjgmUUqptlbs74OgvE2upms

By default the left-most worksheets will be used, we can change this by providing either
`worksheet_id` or `worksheet_name`.

The first time in each session that we use `InteractiveSheet` we will need to give Colab permission
to edit our drive assets on our behalf.

```
import polars as pl
from google.colab import sheets
url = "https://docs.google.com/spreadsheets/d/1BxiMVs0XRA5nFMdKvBdBZjgmUUqptlbs74OgvE2upms"
sheet = sheets.InteractiveSheet(url=url, backend="polars", display=False)
sheet.as_df()
```
## Creating a new sheet

When you don't provide the source of the spreadsheet one will be created for you.

```
sheet = sheets.InteractiveSheet(title="Colab <3 Polars", backend="polars")
```
When you pass the `df` parameter the data will be written to the sheet immediately.

```
df = pl.DataFrame({"a": [1,2,3], "b": ["a", "b", "c"]})
sheet = sheets.InteractiveSheet(df=df, title="Colab <3 Polars", backend="polars")
```
## Writing to a sheet

By default the `update` method will clear the worksheet and write the dataframe in the top left
corner.

```
sheet.update(df)
```
We can modify where the data is written with the `location` parameter and whether the worksheet is
cleared before with `clear`.

```
sheet.update(df, clear=False)
sheet.update(df, location="D3")
sheet.update(df, location=(3, 4))
```
A good way to write multiple dataframes onto a worksheet in a loop is:

```
for i, df in dfs:
  df = pl.select(x=pl.arange(5)).with_columns(pow=pl.col("x") ** i)
  sheet.update(df, loc=(1, i * 3), clear=i == 0)
```
This clears the worksheet then writes the dataframes next to each other, one every five columns.

# Citations

1. Source page: https://docs.pola.rs/user-guide/io/sheets_colab
