# Snapshot column rename

A snapshot keeps one historical column when the source renames a field. The timestamp strategy does not depend on the column list.

## Look for

- `strategy: check` with `check_cols: all` after a source column was renamed. Every row looks new, and the old column is left behind.
- A rebuild of the snapshot to pick up the new name, which drops history the snapshot cannot recompute.
- Downstream models that read only the new column and ignore the old one.

## Fix shape

- Prefer `strategy: timestamp` when `updated_at` is reliable, so a new column does not require a new `check_cols` list.
- If the rename is the same attribute, coalesce the old and new columns in the model downstream of the snapshot. Do not drop the snapshot table.

## Field reports

- [Upstream schema changes and snapshots](https://www.reddit.com/r/dataengineering/comments/1i8sbzm/dbt_how_do_you_handle_upstream_schema_changes/) — r/dataengineering. A renamed source column threatened to break the snapshot or duplicate history. The reply that matches the docs is to keep the snapshot and coalesce the two names downstream when the rename has the same meaning.

## Canonical check

- [dbt snapshots](https://docs.getdbt.com/docs/build/snapshots) — The timestamp strategy uses `updated_at` and is the one dbt recommends when columns are added or removed. The check strategy compares `check_cols`. Snapshots are not rebuilt from scratch without losing the history they stored.

Coalescing is wrong when the new column is a different attribute, not a rename. In that case it is a new column and the old history stays on the old name.
