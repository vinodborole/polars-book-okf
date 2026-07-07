---
type: Web Page
title: Google BigQuery - Polars user guide
resource: https://docs.pola.rs/user-guide/io/bigquery
timestamp: '2026-07-07T12:26:19.464100+00:00'
---

# Google BigQuery

To read or write from GBQ, additional dependencies are needed:

```
$ pip install google-cloud-bigquery
```
## Read

We can load a query into a `DataFrame` like this:

  `from_arrow` ·  Available on feature fsspec ·  Available on feature pyarrow

```
import polars as pl
from google.cloud import bigquery
client = bigquery.Client()
# Perform a query.
QUERY = (
    'SELECT name FROM `bigquery-public-data.usa_names.usa_1910_2013` '
    'WHERE state = "TX" '
    'LIMIT 100')
query_job = client.query(QUERY)  # API request
rows = query_job.result()  # Waits for query to finish
df = pl.from_arrow(rows.to_arrow())
```
## Write

```
from google.cloud import bigquery
client = bigquery.Client()
# Write DataFrame to stream as parquet file; does not hit disk
with io.BytesIO() as stream:
    df.write_parquet(stream)
    stream.seek(0)
    parquet_options = bigquery.ParquetOptions()
    parquet_options.enable_list_inference = True
    job = client.load_table_from_file(
        stream,
        destination='tablename',
        project='projectname',
        job_config=bigquery.LoadJobConfig(
            source_format=bigquery.SourceFormat.PARQUET,
            parquet_options=parquet_options,
        ),
    )
job.result()  # Waits for the job to complete
```

# Citations

1. Source page: https://docs.pola.rs/user-guide/io/bigquery
