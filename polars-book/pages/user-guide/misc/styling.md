---
type: Web Page
title: Styling - Polars user guide
resource: https://docs.pola.rs/user-guide/misc/styling
timestamp: '2026-07-27T09:55:25.099201+00:00'
---

# Styling

Data in a Polars `DataFrame` can be styled for presentation use the `DataFrame.style` property. This
returns a `GT` object from
[Great Tables](https://posit-dev.github.io/great-tables/articles/intro.html), which enables
structuring, formatting, and styling for table display.

```
import polars as pl
import polars.selectors as cs
path = "docs/assets/data/iris.csv"
df = (
    pl.scan_csv(path)
    .group_by("species")
    .agg(cs.starts_with("petal").mean().round(3))
    .collect()
)
print(df)
```
```
shape: (3, 3)
┌────────────┬──────────────┬─────────────┐
│ species    ┆ petal_length ┆ petal_width │
│ ---        ┆ ---          ┆ ---         │
│ str        ┆ f64          ┆ f64         │
╞════════════╪══════════════╪═════════════╡
│ Versicolor ┆ 4.26         ┆ 1.326       │
│ Setosa     ┆ 1.462        ┆ 0.246       │
│ Virginica  ┆ 5.552        ┆ 2.026       │
└────────────┴──────────────┴─────────────┘
```
## Structure: add header title

```
df.style.tab_header(title="Iris Data", subtitle="Mean measurement values per species")
```
| Iris Data | ||
| Mean measurement values per species | ||
| species | petal_length | petal_width | 
|---|---|---|
| Versicolor | 4.26 | 1.326 | 
| Setosa | 1.462 | 0.246 | 
| Virginica | 5.552 | 2.026 | 

## Structure: add row stub

```
df.style.tab_stub(rowname_col="species")
```
| petal_length | petal_width | |
|---|---|---|
| Versicolor | 4.26 | 1.326 | 
| Setosa | 1.462 | 0.246 | 
| Virginica | 5.552 | 2.026 | 

## Structure: add column spanner

```
(
    df.style.tab_spanner("Petal", cs.starts_with("petal")).cols_label(
        petal_length="Length", petal_width="Width"
    )
)
```
| species | Petal | |
|---|---|---|
| Length | Width | |
| Versicolor | 4.26 | 1.326 | 
| Setosa | 1.462 | 0.246 | 
| Virginica | 5.552 | 2.026 | 

## Format: limit decimal places

```
df.style.fmt_number("petal_width", decimals=1)
```
| species | petal_length | petal_width | 
|---|---|---|
| Versicolor | 4.26 | 1.3 | 
| Setosa | 1.462 | 0.2 | 
| Virginica | 5.552 | 2.0 | 

## Style: highlight max row

```
from great_tables import loc, style
df.style.tab_style(
    style.fill("yellow"),
    loc.body(
        rows=pl.col("petal_length") == pl.col("petal_length").max(),
    ),
)
```
| species | petal_length | petal_width | 
|---|---|---|
| Versicolor | 4.26 | 1.326 | 
| Setosa | 1.462 | 0.246 | 
| Virginica | 5.552 | 2.026 | 

## Style: bold species column

```
from great_tables import loc, style
df.style.tab_style(
    style.text(weight="bold"),
    loc.body(columns="species"),
)
```
| species | petal_length | petal_width | 
|---|---|---|
| Versicolor | 4.26 | 1.326 | 
| Setosa | 1.462 | 0.246 | 
| Virginica | 5.552 | 2.026 | 

## Full example

```
from great_tables import loc, style
(
    df.style.tab_header(
        title="Iris Data", subtitle="Mean measurement values per species"
    )
    .tab_stub(rowname_col="species")
    .cols_label(petal_length="Length", petal_width="Width")
    .tab_spanner("Petal", cs.starts_with("petal"))
    .fmt_number("petal_width", decimals=2)
    .tab_style(
        style.fill("yellow"),
        loc.body(
            rows=pl.col("petal_length") == pl.col("petal_length").max(),
        ),
    )
)
```
| Iris Data | ||
| Mean measurement values per species | ||
| Petal | ||
|---|---|---|
| Length | Width | |
| Versicolor | 4.26 | 1.33 | 
| Setosa | 1.462 | 0.25 | 
| Virginica | 5.552 | 2.03 |

# Citations

1. Source page: https://docs.pola.rs/user-guide/misc/styling
