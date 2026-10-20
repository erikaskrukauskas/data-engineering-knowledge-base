# Seed full refresh cascade

`dbt seed --full-refresh` drops the seed table with cascade. Downstream warehouse objects on that table can be dropped with it.

## Look for

- `--full-refresh` on every seed run.
- A BI view created in the warehouse on top of the seed, missing after a column change.
- A normal value change loaded with full refresh instead of truncate-and-insert.

## Fix shape

- Full-refresh only when columns changed.
- Rebuild the dropped dependents, or define them as dbt models so the next build recreates them.

## Field reports

No thread in the catalog communities stated this failure with a stack and a fix that matches the docs. Search [r/dataengineering](https://www.reddit.com/r/dataengineering/) and the tool tags in `sources/catalog.yaml` before treating a comment as a rule.

## Canonical check

- [Seeds](https://docs.getdbt.com/docs/build/seeds) — The normal command truncates and reinserts to avoid `drop cascade`. `--full-refresh` drops the table with cascade, which can drop objects BI users are querying.

The column-mismatch error is the signal to full-refresh. A successful normal seed does not need it.
