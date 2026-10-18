# Snapshot schema

A snapshot relation is not rebuilt from the source. It is stored apart from the models that get dropped and recreated.

## Look for

- Snapshots materialized into the mart schema that a cleanup job drops.
- A snapshot model edited to change history and then `--full-refresh`ed.
- `hard_deletes` left at `ignore` when a source delete should close the row.

## Fix shape

- Give snapshots a schema that model cleanup does not drop.
- Use `invalidate` or `new_record` when a delete must end the open version.
- Prefer `strategy: timestamp` when `updated_at` is reliable.

## Field reports

No thread in the catalog communities stated this failure with a stack and a fix that matches the docs. Search [r/dataengineering](https://www.reddit.com/r/dataengineering/) and the tool tags in `sources/catalog.yaml` before treating a comment as a rule.

## Canonical check

- [dbt snapshots](https://docs.getdbt.com/docs/build/snapshots) — `hard_deletes` defaults to `ignore`. `invalidate` and `new_record` record the delete. The timestamp strategy is the one that survives column adds. The snapshot table is the history.

Contracts do not apply to snapshots. A column rename is the snapshot-column-rename note, not a contract failure.
