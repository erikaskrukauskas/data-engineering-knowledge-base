# Cumulative time spine

A cumulative metric is added only after the time spine model exists.

## Look for

- `type: cumulative` with `window` and no time spine model in the project.
- `join_to_timespine: true` on a simple metric with no spine.
- An infinite window, which is what you get when `window` is omitted, on a metric that was supposed to be seven days.

## Fix shape

- Create the time spine before the cumulative metric.
- Set `window` or `grain_to_date`, not both.
- Set `join_to_timespine` and `fill_nulls_with` only when empty periods should become zeros.

## Field reports

No thread in the catalog communities stated this failure with a stack and a fix that matches the docs. Search [r/dataengineering](https://www.reddit.com/r/dataengineering/) and the tool tags in `sources/catalog.yaml` before treating a comment as a rule.

## Canonical check

- [Creating metrics](https://docs.getdbt.com/docs/build/metrics-overview) — A cumulative metric needs the time spine first. Omitting `window` accumulates all recorded time. `window` and `grain_to_date` are alternatives.

`period_agg` of first, last, or average changes which value represents the period. It does not create the spine.
