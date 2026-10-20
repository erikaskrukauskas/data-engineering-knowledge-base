# Semantic aggregation time

A semantic model that defines metrics sets `agg_time_dimension` to a time dimension on that model.

## Look for

- `semantic_model:` enabled with `metrics:` and no `agg_time_dimension`.
- A metric that aggregates on a timestamp column that is not declared as a time dimension.
- Two metrics that should use different clocks, with only the model-level default set.

## Fix shape

- Set `agg_time_dimension` on the model.
- Override `agg_time_dimension` on the metric that uses a different time column.

## Field reports

No thread in the catalog communities stated this failure with a stack and a fix that matches the docs. Search [r/dataengineering](https://www.reddit.com/r/dataengineering/) and the tool tags in `sources/catalog.yaml` before treating a comment as a rule.

## Canonical check

- [Semantic models](https://docs.getdbt.com/docs/build/semantic-models) — `agg_time_dimension` is required. Metrics inherit it, and a simple metric can override it.
- [Creating metrics](https://docs.getdbt.com/docs/build/metrics-overview) — The native grain is set on the time column. The rollup grain is chosen at query time.

The latest YAML spec applies to dbt v1.12, dbt v2, and the platform latest track. Older projects still use the legacy standalone semantic-model YAML.
