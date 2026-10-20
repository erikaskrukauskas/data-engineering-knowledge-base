# Metric filter selection

`dbt list --select <semantic_model>+` includes metrics that use that model's simple or derived metrics. A metric that only filters on the model's dimension is not in that selection.

## Look for

- A CI selector `state:modified+` expected to rebuild every metric that mentions a changed dimension in a filter.
- A metric whose only link to a model is `filter: Dimension(...)`.
- A dashboard that breaks after a dimension rename the selector did not build.

## Fix shape

- Select the metric by name when the change is in a filter.
- Do not treat a filter reference as a DAG parent.

## Field reports

No thread in the catalog communities stated this failure with a stack and a fix that matches the docs. Search [r/dataengineering](https://www.reddit.com/r/dataengineering/) and the tool tags in `sources/catalog.yaml` before treating a comment as a rule.

## Canonical check

- [Semantic models](https://docs.getdbt.com/docs/build/semantic-models) — Dependencies from filters are not reflected in selection syntax or in the dbt docs graph. A metric is downstream only when it uses the model's metrics.

The model build graph is unchanged. This gap is the metric graph, not `ref()`.
