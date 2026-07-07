---
type: Web Page
title: Getting started - Polars user guide
resource: https://docs.pola.rs/polars-cloud/quickstart
timestamp: '2026-07-07T12:26:19.464100+00:00'
---

# Getting started

Polars Cloud is a managed compute platform for your Polars queries. It allows you to effortlessly run your local queries in your cloud environment without infrastructure management. By working in a 'Bring your own Cloud' model the data never leaves your environment.

## Installation

Install the Polars Cloud python library in your environment

```
$ pip install polars polars-cloud
```
Create an account and login by running the command below.

```
$ pc authenticate
```
## Connect your cloud

Polars Cloud currently exclusively supports AWS as a cloud provider.

Polars Cloud needs permission to manage hardware in your environment. This is done by deploying our cloudformation template. See our infrastructure section for more details.

To set up your Polars Cloud environment and connect your cloud run you can either

- Run `pc setup`to guide you through creation and connecting via CLI.
- Or create an organization and workspace via the browser.

## Run your queries

Now that we are done with the setup, we can start running queries. You can write Polars like you're
used and to only need to call `.remote()` on your `LazyFrame`. In the following example we create a
compute cluster and run a simple Polars query.

  `ComputeContext` ·  `LazyFrameRemote`

```
import polars as pl
import polars_cloud as pc
# First, we need to define the hardware the cluster will run on.
# This can be done by specifying the minimum CPU and memory or
# by specifying the exact instance type in AWS.
ctx = pc.ComputeContext(memory=8, cpus=2, cluster_size=1)
# Then we write a regular lazy Polars query. In this example
# we compute the maximum of column.
lf = pl.LazyFrame(
    {
        "a": [1, 2, 3],
        "b": [4, 4, 5],
    }
).with_columns(
    pl.col("a").max().over("b").alias("c"),
)
# At this point, the query has not been executed yet.
# We need to call `.remote()` to signal that we want to run
# on Polars Cloud and then `.execute()` send the query and execute it.
lf.remote(context=ctx).execute()
# The query and compute used will also show up in the
# portal at https://cloud.pola.rs/portal/
```

# Citations

1. Source page: https://docs.pola.rs/polars-cloud/quickstart
