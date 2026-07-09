---
type: Web Page
title: Execute remote query - Polars user guide
resource: https://docs.pola.rs/polars-cloud/run/remote-query
timestamp: '2026-07-09T12:17:10.704938+00:00'
---

# Execute remote query

Polars Cloud enables you to execute existing Polars queries on cloud infrastructure with minimal code changes. This approach allows you to process datasets that exceed local resources or use additional compute resources for faster execution.

Polars Cloud is set up and connected

This page assumes that you have created an organization and connected a workspace to your cloud environment. If you haven't yet, follow the steps on the [Connect cloud environment](../../connect-cloud/) page.

## Define your query locally

The following example uses a query from the PDS-H benchmark suite, a derived version of the popular
TPC-H benchmark. Data generation tools and additional queries are available in the
[Polars benchmark repository](https://github.com/pola-rs/polars-benchmark).

```
import polars as pl
customer = pl.scan_parquet("data/customer.parquet")
lineitem = pl.scan_parquet("data/lineitem.parquet")
orders = pl.scan_parquet("data/orders.parquet")
def pdsh_q3(
    customer: pl.LazyFrame, lineitem: pl.LazyFrame, orders: pl.LazyFrame
) -> pl.LazyFrame:
    return (
        customer.filter(pl.col("c_mktsegment") == "BUILDING")
        .join(orders, left_on="c_custkey", right_on="o_custkey")
        .join(lineitem, left_on="o_orderkey", right_on="l_orderkey")
        .filter(pl.col("o_orderdate") < pl.date(1995, 3, 15))
        .filter(pl.col("l_shipdate") > pl.date(1995, 3, 15))
        .with_columns(
            (pl.col("l_extendedprice") * (1 - pl.col("l_discount"))).alias("revenue")
        )
        .group_by("o_orderkey", "o_orderdate", "o_shippriority")
        .agg(pl.sum("revenue"))
        .select(
            pl.col("o_orderkey").alias("l_orderkey"),
            "revenue",
            "o_orderdate",
            "o_shippriority",
        )
        .sort(by=["revenue", "o_orderdate"], descending=[True, False])
    )
pdsh_q3(customer, lineitem, orders).collect()
```
## Scale to the cloud

To execute your query in the cloud, you need to define a compute context. The compute context
specifies the hardware to use when executing the query in the cloud. It allows you to set the
workspace to execute your query and set compute resources. More elaborate options can be found on
the [Compute context introduction page](../../context/compute-context/).

```
import polars_cloud as pc
ctx = pc.ComputeContext(
    # make sure to enter your own workspace name
    workspace="your-workspace",
    memory=16,
    cpus=12,
)
# Use a larger dataset available on S3
lineitem_sf10 = pl.scan_parquet(
    "s3://polars-cloud-samples-us-east-2-prd/pdsh/sf10/lineitem.parquet",
    storage_options={"request_payer": "true"},
)
customer_sf10 = pl.scan_parquet(
    "s3://polars-cloud-samples-us-east-2-prd/pdsh/sf10/customer.parquet",
    storage_options={"request_payer": "true"},
)
orders_sf10 = pl.scan_parquet(
    "s3://polars-cloud-samples-us-east-2-prd/pdsh/sf10/orders.parquet",
    storage_options={"request_payer": "true"},
)
# Your query remains the same
pdsh_q3(customer_sf10, lineitem_sf10, orders_sf10).remote(context=ctx).show()
```
Run the examples yourself

All examples on this page can be executed using the sample datasets hosted on our S3 bucket. By including the `storage_option` parameter in your queries, you'll only incur S3 data transfer costs. No additional storage fees apply

S3 bucket region

The example datasets are hosted in the `us-east-2 S3 region`. Query performance may be affected if you're running operations from a distant geographic location due to network latency.

## Working with remote query results

Once you've called `.remote(context=ctx)` on your query, you have several options for how to handle
the results, each suited to different use cases and workflows.

### Write to storage

The most straightforward approach for batch processing is to write results directly to cloud storage
using `.sink_parquet()`. This method is ideal when you want to store processed data for later use or
as part of a data pipeline:

```
# Replace the S3 url with your own to run the query successfully
(
    pdsh_q3(customer_sf10, lineitem_sf10, orders_sf10)
    .remote(context=ctx)
    .sink_parquet("s3://your-bucket/processed-data/")
)
```
Running `.sink_parquet()` will write the results to the defined bucket on S3. The query you execute
runs in your cloud environment, and both the data and results remain secure in your own
infrastructure. This approach is perfect for ETL workflows, scheduled jobs, or any time you need to
persist large datasets without transferring them to your local machine.

### Inspect results

Using `.show()` will display the first 10 rows of the result so you can inspect the structure
without having to transfer the whole dataset. This method displays the first 10 rows in your console
or notebook.

```
pdsh_q3(customer_sf10, lineitem_sf10, orders_sf10).remote(context=ctx).show()
```
```
shape: (10, 4)
┌────────────┬─────────────┬─────────────┬────────────────┐
│ l_orderkey ┆ revenue     ┆ o_orderdate ┆ o_shippriority │
│ ---        ┆ ---         ┆ ---         ┆ ---            │
│ i64        ┆ f64         ┆ date        ┆ i64            │
╞════════════╪═════════════╪═════════════╪════════════════╡
│ 4791171    ┆ 440715.2185 ┆ 1995-02-23  ┆ 0              │
│ 46678469   ┆ 439855.325  ┆ 1995-01-27  ┆ 0              │
│ 23906758   ┆ 432728.5737 ┆ 1995-03-14  ┆ 0              │
│ 23861382   ┆ 428739.1368 ┆ 1995-03-09  ┆ 0              │
│ 59393639   ┆ 426036.0662 ┆ 1995-02-12  ┆ 0              │
│ 3355202    ┆ 425100.6657 ┆ 1995-03-04  ┆ 0              │
│ 9806272    ┆ 425088.0568 ┆ 1995-03-13  ┆ 0              │
│ 22810436   ┆ 423231.969  ┆ 1995-01-02  ┆ 0              │
│ 16384100   ┆ 421478.7294 ┆ 1995-03-02  ┆ 0              │
│ 52974151   ┆ 415367.1195 ┆ 1995-02-05  ┆ 0              │
└────────────┴─────────────┴─────────────┴────────────────┘
```
The `.await_and_scan()` method returns a LazyFrame pointing to intermediate results stored
temporarily in your S3 environment. These intermediate result files are automatically deleted after
several hours. For persistent storage use `sink_parquet`. The output is a LazyFrame, allowing
continued query chaining for further analysis.

```
result = (
    pdsh_q3(customer_sf10, lineitem_sf10, orders_sf10)
    .remote(context=ctx)
    .await_and_scan()
)
print(result.collect())
```
```
shape: (114_003, 4)
┌────────────┬─────────────┬─────────────┬────────────────┐
│ l_orderkey ┆ revenue     ┆ o_orderdate ┆ o_shippriority │
│ ---        ┆ ---         ┆ ---         ┆ ---            │
│ i64        ┆ f64         ┆ date        ┆ i64            │
╞════════════╪═════════════╪═════════════╪════════════════╡
│ 4791171    ┆ 440715.2185 ┆ 1995-02-23  ┆ 0              │
│ 46678469   ┆ 439855.325  ┆ 1995-01-27  ┆ 0              │
│ 23906758   ┆ 432728.5737 ┆ 1995-03-14  ┆ 0              │
│ 23861382   ┆ 428739.1368 ┆ 1995-03-09  ┆ 0              │
│ 59393639   ┆ 426036.0662 ┆ 1995-02-12  ┆ 0              │
│ …          ┆ …           ┆ …           ┆ …              │
│ 44149381   ┆ 904.3968    ┆ 1995-01-16  ┆ 0              │
│ 34297697   ┆ 897.8464    ┆ 1995-03-06  ┆ 0              │
│ 25478115   ┆ 887.2318    ┆ 1994-11-28  ┆ 0              │
│ 52204674   ┆ 860.25      ┆ 1994-12-18  ┆ 0              │
│ 47255457   ┆ 838.9381    ┆ 1994-11-18  ┆ 0              │
└────────────┴─────────────┴─────────────┴────────────────┘
```

# Citations

1. Source page: https://docs.pola.rs/polars-cloud/run/remote-query
