# owid-grapher-py

_Experimental grapher package for use in Jupyter notebooks._

## Status

This package relies on internal APIs that may not be supported in future, so please consider it experimental.

## Installing

```
pip install git+https://github.com/owid/owid-grapher-py
```

## How to use

Get your data into a tidy data frame, then wrap it in a chart object and explain what marks you want and how to encode the dimensions you have (inspired by Altair).

```python
from owid.grapher import Chart

Chart(df).mark_line().encode(x='year', y='population').label('Too many Koalas?')
```

### Chart types

You specify your chart type with one of:

- `mark_line()`
- `mark_bar()`

#### Line chart

```python
# a third dimension can be encoded in the color
Chart(df2).mark_line().encode(
    x='year', y='population', c='region'
).label(title='Population by region')
```

#### Bar chart

```python
# regular bar chart
Chart(df).mark_bar().encode(x='population', y='region')
```

```python
# stacked bar chart
Chart(df2).mark_bar(stacked=True).encode(
    x='energy_generated',
    y='country',
    c='energy_source'
)
```

### Labels

```python
Chart(df).encode(
    x='year', y='population'
).label(
    title='Very important',
    subtitle='Less important',
    note='An extra note',
    source_desc='This data was randomly generated'
)
```

### Transforms

```python
# plot relative change
Chart(...).transform(relative=True)
```

### Interactivity

```python
# enable relative mode toggle
Chart(...).interact(relative=True)
```

```python
# let you switch between log and linear scale
Chart(...).interact(scale_control=True)
```

```python
# enable the country/entity picker
Chart(...).interact(entity_control=True)
```

```python
# enable the map tab
Chart(...).interact(enable_map=True)
```

### Data selection

```python
Chart(...).select(
    entities=['Australia', 'New Zealand'],
    timespan=(1990, 2020)
)
```

## How it works

OWID's grapher JS library has an internal JSON config format that all charts are created from. When you create a Python chart object, you are building up enough information to make a JSON config. You can see the config that gets generated by typing `.export()` on one of your chart objects in the notebook.

When Jupyter asks to display the chart object, it calls its `_repr_html_()` method, and the chart return an html snippet containing an iframe and some js to inject dynamic iframe contents. This makes an iframe equivalent to a pre-prepared chart page on the Our World In Data site. Neat, huh?

## TODO

This project should not attempt feature parity with grapher, but should walk the line between
making an expressive charting tool and making something that can reproduce a large percentage of
our existing charts. Some ideas for improvement:

Enable `grapher.Chart()` to support more chart types:

- [ ] Scatterplots
- [ ] Axis bounds
- [ ] Line charts without a time axis

Auto-generate more types of notebooks correctly

- [ ] Multi-variable single entity line-charts
- [ ] Bar charts
- [ ] Stacked bar charts
- [ ] Time selection

## Changelog

- `0.1.3`
    - Do not render the data when auto-generating notebooks
    - Allow fetching data by slug
    - Allow fetching data and config from dev environments
- `0.1.2`
    - Support timespans with `select()`
- `0.1.1`
    - Improve `select()`, `interact()` and `label()` methods on `Chart` to work in more cases
    - Helpers in to download the config or the data from a chart page (`owid.site`)
    - Generate a notebook with python plotting commands for a subset of charts (`owid.grapher.notebook`)
- `0.1.0`
    - Plot basic line charts, bar charts and stacked bar charts in the notebook
