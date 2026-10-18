# Loaded-at query

`loaded_at_query` is the source of the timestamp when the table has no reliable loaded-at column.

## Look for

- `loaded_at_field` pointed at an updated-at that the source rewrites for old rows.
- A lookback in an incremental model and a freshness query that does not use the same window.
- `loaded_at_query` that scans the whole table on every freshness run.

## Fix shape

- Point the query at the watermark the loader writes.
- Keep the incremental lookback and the freshness query on the same definition of late.

## Field reports

No thread in the catalog communities stated this failure with a stack and a fix that matches the docs. Search [r/dataengineering](https://www.reddit.com/r/dataengineering/) and the tool tags in `sources/catalog.yaml` before treating a comment as a rule.

## Canonical check

- [Freshness config](https://docs.getdbt.com/reference/resource-configs/freshness) — `loaded_at_query` replaces `loaded_at_field` when you need custom SQL for the timestamp.
- [Incremental models](https://docs.getdbt.com/docs/build/incremental-models) — A lookback that ignores rows the source marked late will skip them. The loaded-at definition has to match.

A freshness query runs on the cadence of the freshness job. A full scan in that query is a cost bug even when the threshold is right.
