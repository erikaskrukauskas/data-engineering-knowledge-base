# Simple metric aggregation

A simple metric sets `type: simple` and `agg`. `expr` defaults to the metric name when it is omitted.

## Look for

- A simple metric with no `agg`.
- `expr` omitted while the metric name does not match the column.
- `agg: percentile` without `percentile` and `percentile_type`.

## Fix shape

- Set `agg` to one of sum, max, min, average, median, count_distinct, percentile, count, or sum_boolean.
- Set `expr` to the column. For percentile, set `percentile` and `percentile_type` of discrete or continuous.

## Field reports

No thread in the catalog communities stated this failure with a stack and a fix that matches the docs. Search [r/dataengineering](https://www.reddit.com/r/dataengineering/) and the tool tags in `sources/catalog.yaml` before treating a comment as a rule.

## Canonical check

- [Creating metrics](https://docs.getdbt.com/docs/build/metrics-overview) — `agg` is required on a simple metric. `expr` defaults to the metric name. Percentile requires `percentile` and `percentile_type`.

`count` with `expr: 1` counts rows. `count_distinct` counts a column. Swapping them changes the metric without a warehouse error.
