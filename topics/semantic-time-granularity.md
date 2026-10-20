# Time dimension granularity

A time dimension sets `granularity` on the column. Queries roll up only to that grain or a coarser one.

## Look for

- `dimension.type: time` with no `granularity`.
- A metric spec that stores `time_granularity: hour` on a column whose native grain is day.
- A query that asks for hour on a daily column.

## Fix shape

- Set `granularity` to the grain stored in the column.
- Choose a coarser rollup in the query, not a finer one in the metric YAML.

## Field reports

No thread in the catalog communities stated this failure with a stack and a fix that matches the docs. Search [r/dataengineering](https://www.reddit.com/r/dataengineering/) and the tool tags in `sources/catalog.yaml` before treating a comment as a rule.

## Canonical check

- [Creating metrics](https://docs.getdbt.com/docs/build/metrics-overview) — Native grain lives on the column. A query can roll up to a grain equal to or coarser than that native grain.
- [Semantic models](https://docs.getdbt.com/docs/build/semantic-models) — Time dimensions require a column-level granularity.

This is the v1.12 spec. A legacy semantic YAML file uses its own keys. Do not mix the two shapes in one model.
