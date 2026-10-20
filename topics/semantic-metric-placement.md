# Metric YAML placement

Simple metrics are a `metrics` list beside `semantic_model`. Cumulative, ratio, derived, and conversion metrics that use another semantic model are a top-level `metrics` block.

## Look for

- A simple metric in the top-level `metrics:` list. The metrics page says simple metrics cannot be defined there.
- A ratio or cumulative metric nested on one model when its inputs come from two semantic models.
- `metrics` nested inside the `semantic_model:` mapping.

## Fix shape

- Put simple metrics on the model, next to `semantic_model`.
- Put a metric that references another semantic model in the top-level `metrics` list.

## Field reports

No thread in the catalog communities stated this failure with a stack and a fix that matches the docs. Search [r/dataengineering](https://www.reddit.com/r/dataengineering/) and the tool tags in `sources/catalog.yaml` before treating a comment as a rule.

## Canonical check

- [Creating metrics](https://docs.getdbt.com/docs/build/metrics-overview) — Simple metrics belong with the semantic model. Advanced metrics that reference other semantic models belong in a top-level `metrics` key. Simple metrics cannot be top-level.
- [Metric properties](https://docs.getdbt.com/reference/metric-properties) — Metrics are not nested under `semantic_model`. Cross-model metrics go outside `models:`.

The metrics overview also describes the location as under `semantic_model`. The YAML examples and the properties page put `metrics` beside `semantic_model`. Follow the examples when the two sentences disagree.
