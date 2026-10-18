# Incremental loads and backfill

An incremental model can rebuild a past window without duplicating rows. The window is the business event date, not the date the row was extracted.

## Look for

- `insert_overwrite` or a replace filter keyed by extraction time, so a late row lands in a new partition and the original business date is never replaced.
- `full_refresh` as the only way to repair history.
- An Airflow backfill that does not pass the same start and end variables the dbt model uses.
- A `unique_key` that is not unique for the grain, or a `unique_key` column that can be null. Nulls do not match, so the same entity is inserted again. `merge` and `delete+insert` use this key. `insert_overwrite` does not: it replaces a partition.
- `on_schema_change` omitted or set to `ignore`. A newly selected column never appears in the table. Removing a column fails the run. None of the settings backfill the new column onto old rows.
- A model `state.lag_tolerance` whose source has no `loaded_at_query` covering the same lookback. dbt State can skip the model while late rows sit inside the window the incremental filter would have read.

## Fix shape

- Parameterize the window in the DAG and in the model. A daily run and a backfill use that same parameter.
- Replace the target rows for that window (`insert_overwrite`, `delete+insert`, or `merge` on a real grain) before inserting the rebuilt rows.
- Keep a separate, explicit backfill invocation. Do not overload the scheduled run with a silent full refresh.

## Field reports

- [dbt incremental models with insert_overwrite: backfill causing duplicates](https://www.reddit.com/r/dataengineering/comments/1l1lpdz/dbt_incremental_models_with_insert_overwrite/) — r/dataengineering. Late rows extracted on a later day duplicated because the overwrite partition was the extract date.
- [Incremental loading, backfill, idempotence, and source deletions](https://www.reddit.com/r/dataengineering/comments/13n60lq/are_there_common_approaches_to_designing_a/) — r/dataengineering
- Tag page: [`dbt` sorted by votes](https://stackoverflow.com/questions/tagged/dbt?tab=Votes), especially `incremental_strategy` and `full_refresh`

## Canonical check

- [dbt incremental models](https://docs.getdbt.com/docs/build/incremental-models)
- [Idempotence in dbt](https://docs.getdbt.com/best-practices/idempotence)
- [Incremental models in depth](https://docs.getdbt.com/best-practices/materializations/4-incremental-models) — lookback windows for late rows, and why a `unique_key` has to accompany that window
- [Airflow best practices](https://airflow.apache.org/docs/apache-airflow/stable/best-practices.html) on reading and writing a specific data interval

Confirm the strategy against the adapter in use. BigQuery, Snowflake, Spark, and Databricks do not implement `insert_overwrite` the same way. On Snowflake, `dbt-snowflake` below 1.10.6 can fail an incremental model that uses `on_schema_change: sync_all_columns` on a string column with collation. `on_schema_change` only sees top-level columns, so a nested field can change without triggering it.
