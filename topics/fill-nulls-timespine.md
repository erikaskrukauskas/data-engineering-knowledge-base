# Time spine nulls

`join_to_timespine` and `fill_nulls_with` are set when a period with no rows should appear as a number instead of disappearing.

## Look for

- A weekly chart that skips weeks with no orders.
- `fill_nulls_with: 0` without `join_to_timespine`.
- A cumulative metric with a window and no spine model in the project.

## Fix shape

- Create the time spine.
- Set `join_to_timespine: true` and `fill_nulls_with` to the value empty periods should show.

## Field reports

No thread in the catalog communities stated this failure with a stack and a fix that matches the docs. Search [r/dataengineering](https://www.reddit.com/r/dataengineering/) and the tool tags in `sources/catalog.yaml` before treating a comment as a rule.

## Canonical check

- [Creating metrics](https://docs.getdbt.com/docs/build/metrics-overview) — Simple and cumulative metrics can set `join_to_timespine` and `fill_nulls_with`. Cumulative metrics require the time spine model first.

Filling nulls changes the chart. It does not change how many source rows qualified. A ratio can still move because the denominator gained periods.
