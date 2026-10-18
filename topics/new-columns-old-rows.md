# New columns on old rows

`on_schema_change` can add a column. None of its settings fill that column on rows already stored.

## Look for

- `on_schema_change: append_new_columns` or `sync_all_columns` expected to populate history.
- `ignore`, the default, which drops the new column from the incremental write.
- A nested field added inside a struct. dbt does not track those as column changes.
- A contract plus `ignore` or `sync_all_columns` on an incremental model.

## Fix shape

- Use `append_new_columns` or `fail` when the model is contracted.
- Backfill the new column with an explicit update or a bounded full refresh of the partitions you mean to rewrite.

## Field reports

No thread in the catalog communities stated this failure with a stack and a fix that matches the docs. Search [r/dataengineering](https://www.reddit.com/r/dataengineering/) and the tool tags in `sources/catalog.yaml` before treating a comment as a rule.

## Canonical check

- [Incremental models](https://docs.getdbt.com/docs/build/incremental-models) — The default `on_schema_change` is `ignore`. The other settings change the table schema. They do not backfill new columns onto old rows. Nested fields are not tracked.
- [Model contracts](https://docs.getdbt.com/docs/mesh/govern/model-contracts) — A contracted incremental model allows `append_new_columns` or `fail`.

On Snowflake, `sync_all_columns` can fail on collated strings with dbt-snowflake before 1.10.6. Prefer `append_new_columns` unless you have read that adapter note for the version you run.
