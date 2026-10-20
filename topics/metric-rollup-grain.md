# Metric rollup grain

The column `granularity` is the finest grain. A query rolls up to that grain or a coarser one.

## Look for

- `granularity: day` queried as hour.
- A metric YAML that sets an hourly rollup on a daily column.
- `agg_time_dimension` pointed at a column with no granularity.

## Fix shape

- Store the finer grain on the column if the query needs it.
- Ask for day, week, month, or year only when the column grain is at least that fine.

## Field reports

No thread in the catalog communities stated this failure with a stack and a fix that matches the docs. Search [r/dataengineering](https://www.reddit.com/r/dataengineering/) and the tool tags in `sources/catalog.yaml` before treating a comment as a rule.

## Canonical check

- [Creating metrics](https://docs.getdbt.com/docs/build/metrics-overview) — Roll up only to grains coarser than or equal to the column's native grain. The rollup is chosen at query time.

Overriding `agg_time_dimension` switches the clock. It does not make a daily column hourly.
