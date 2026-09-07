---
type: Web Page
title: Visualization - Polars user guide
resource: https://docs.pola.rs/user-guide/misc/visualization
timestamp: '2026-09-07T11:52:00.647733+00:00'
---

# Visualization

Data in a Polars `DataFrame` can be visualized using common visualization libraries.

We illustrate plotting capabilities using the Iris dataset. We read a CSV and then plot one column against another, colored by a yet another column.

```
import polars as pl
path = "docs/assets/data/iris.csv"
df = pl.read_csv(path)
print(df)
```
```
shape: (150, 5)
┌──────────────┬─────────────┬──────────────┬─────────────┬───────────┐
│ sepal_length ┆ sepal_width ┆ petal_length ┆ petal_width ┆ species   │
│ ---          ┆ ---         ┆ ---          ┆ ---         ┆ ---       │
│ f64          ┆ f64         ┆ f64          ┆ f64         ┆ str       │
╞══════════════╪═════════════╪══════════════╪═════════════╪═══════════╡
│ 5.1          ┆ 3.5         ┆ 1.4          ┆ 0.2         ┆ Setosa    │
│ 4.9          ┆ 3.0         ┆ 1.4          ┆ 0.2         ┆ Setosa    │
│ 4.7          ┆ 3.2         ┆ 1.3          ┆ 0.2         ┆ Setosa    │
│ 4.6          ┆ 3.1         ┆ 1.5          ┆ 0.2         ┆ Setosa    │
│ 5.0          ┆ 3.6         ┆ 1.4          ┆ 0.2         ┆ Setosa    │
│ …            ┆ …           ┆ …            ┆ …           ┆ …         │
│ 6.7          ┆ 3.0         ┆ 5.2          ┆ 2.3         ┆ Virginica │
│ 6.3          ┆ 2.5         ┆ 5.0          ┆ 1.9         ┆ Virginica │
│ 6.5          ┆ 3.0         ┆ 5.2          ┆ 2.0         ┆ Virginica │
│ 6.2          ┆ 3.4         ┆ 5.4          ┆ 2.3         ┆ Virginica │
│ 5.9          ┆ 3.0         ┆ 5.1          ┆ 1.8         ┆ Virginica │
└──────────────┴─────────────┴──────────────┴─────────────┴───────────┘
```
## Built-in plotting with Altair

Polars has a `plot` method to create plots using [Altair](https://altair-viz.github.io/):

```
chart =  (
    df.plot.point(
        x="sepal_width",
        y="sepal_length",
        color="species",
    )
    .properties(width=500, title="Irises")
    .configure_scale(zero=False)
    .configure_axisX(tickMinStep=1)
)
chart.encoding.x.title = "Sepal Width"
chart.encoding.y.title = "Sepal Length"
chart
```
This is shorthand for:

```
import altair as alt
(
    alt.Chart(df).mark_point(tooltip=True).encode(
        x="sepal_length",
        y="sepal_width",
        color="species",
    )
    .properties(width=500)
    .configure_scale(zero=False)
)
```
and is only provided for convenience, and to signal that Altair is known to work well with Polars.

For configuration, we suggest reading
[Chart Configuration](https://altair-viz.github.io/altair-tutorial/notebooks/08-Configuration.html).
For example, you can:

- Change the width/height/title with `.properties(width=500, height=350, title="My amazing plot")` .
- Change the x-axis label rotation with `.configure_axisX(labelAngle=30)` .
- Change the opacity of the points in your scatter plot with `.configure_point(opacity=.5)` .

## hvPlot

If you import `hvplot.polars`, then it registers a `hvplot` method which you can use to create
interactive plots using [hvPlot](https://hvplot.holoviz.org/).

```
import hvplot.polars
df.hvplot.scatter(
    x="sepal_width",
    y="sepal_length",
    by="species",
    width=650,
    title="Irises",
    xlabel='Sepal Width',
    ylabel='Sepal Length',
)
```
## Matplotlib

To create a scatter plot we can pass columns of a `DataFrame` directly to Matplotlib as a `Series`
for each column. Matplotlib does not have explicit support for Polars objects but can accept a
Polars `Series` by converting it to a NumPy array (which is zero-copy for numeric data without null
values).

Note that because the column `'species'` isn't numeric, we need to first convert it to numeric
values so that it can be passed as an argument to `c`.

```
import matplotlib.pyplot as plt
fig, ax = plt.subplots()
ax.scatter(
    x=df["sepal_width"],
    y=df["sepal_length"],
    c=df["species"].cast(pl.Categorical).to_physical(),
)
ax.set_title('Irises')
ax.set_xlabel('Sepal Width')
ax.set_ylabel('Sepal Length')
```
## Plotnine

[Plotnine](https://plotnine.org/) is a reimplementation of ggplot2 in Python, bringing the Grammar
of Graphics to Python users with an interface similar to its R counterpart. It supports Polars
`DataFrame` by internally converting it to a pandas `DataFrame`.

```
from plotnine import ggplot, aes, geom_point, labs
(
    ggplot(df, mapping=aes(x="sepal_width", y="sepal_length", color="species"))
    + geom_point()
    + labs(title="Irises", x="Sepal Width", y="Sepal Length")
)
```
## Seaborn

[Seaborn](https://seaborn.pydata.org/) accepts Pandas `DataFrame`s. Use `DataFrame.to_pandas()` to
convert Polars dataframes to Pandas dataframes. Note however, that Pandas dataframes may not support
all Polars data types.

```
import seaborn as sns
import matplotlib.pyplot as plt
fig, ax = plt.subplots()
sns.scatterplot(
    df,
    x="sepal_width",
    y="sepal_length",
    hue="species",
    ax=ax,
)
ax.set_title('Irises')
ax.set_xlabel('Sepal Width')
ax.set_ylabel('Sepal Length')
```
## Plotly

[Plotly](https://plotly.com/) can accept a Polars `DataFrame` by leveraging:

- [Narwhals](https://narwhals-dev.github.io/narwhals/) , since plotly v6.0.0, and therefore running
  execution natively without any conversion overhead.

```
import plotly.express as px
px.scatter(
    df,
    x="sepal_width",
    y="sepal_length",
    color="species",
    width=650,
    title="Irises",
    labels={'sepal_width': 'Sepal Width', 'sepal_length': 'Sepal Length'}
)
```

# Citations

1. Source page: https://docs.pola.rs/user-guide/misc/visualization
