# Non-additive balances

A semi-additive measure such as a balance sets `non_additive_dimension` so the aggregation across time is min or max, not a sum of snapshots.

## Look for

- `agg: sum` on an account balance with no `non_additive_dimension`.
- `window_agg` set to sum.
- `name` pointing at a column that is not a dimension.
- A group_by list that omits the account entity, so the last snapshot is taken across accounts.

## Fix shape

- Set `non_additive_dimension.name` to the snapshot date dimension and `window_agg` to min or max.
- List the entity names in `group_by` when the last value is per entity.

## Field reports

No thread in the catalog communities stated this failure with a stack and a fix that matches the docs. Search [r/dataengineering](https://www.reddit.com/r/dataengineering/) and the tool tags in `sources/catalog.yaml` before treating a comment as a rule.

## Canonical check

- [Creating metrics](https://docs.getdbt.com/docs/build/metrics-overview) — `non_additive_dimension.name` must be a dimension. `window_agg` is min or max. `group_by` is an optional list of entities.

This does not change the mart's grain. The mart can still store one row per account per day. The metric is what stops a query from summing those days.
