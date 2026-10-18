# Deletes and late data

Source deletes and late-arriving facts are modeled on purpose. An empty extract, a failed file, or a partial load does not delete the target.

## Look for

- A load that treats zero source rows as "replace the target with nothing."
- An incremental model that inserts new keys and never updates or deletes changed ones.
- Hard deletes copied straight from the source with no change record and no soft-delete flag.
- Late facts written into the partition of their arrival date, so the business-date partition stays wrong.
- No snapshot, change feed, or slowly changing dimension when the source can revise history.
- A snapshot that leaves `hard_deletes` unset or sets `ignore` while the source deletes rows. The default keeps the last version current.
- A snapshot `strategy: check` with `check_cols: all` on a source whose columns are added or removed. Every row looks changed. The timestamp strategy, when `updated_at` is reliable, does not need `check_cols` edited for each new column.

## Fix shape

- Land the raw extract as an append. Decide deletes in the transform, from a delete flag, a CDC event, or a trusted full snapshot of that entity.
- Use a dbt snapshot or an SCD Type 2 dimension when history matters. Set `hard_deletes` to `invalidate` or `new_record` when a missing source row should close or record the delete. Use a change feed (database log, Debezium, or warehouse streams) when the source can name the change.
- Rebuild the business-date partition when a late row arrives for that date.
- Keep `created`, `last_modified`, and a deleted flag on entities so a rerun can tell an update from a new row.

## Field reports

- [Incremental loading, backfill, idempotence, and source deletions](https://www.reddit.com/r/dataengineering/comments/13n60lq/are_there_common_approaches_to_designing_a/) — r/dataengineering. The useful caution in the thread is that an empty upload to raw storage is not a source delete.
- Tag pages: [`dbt`](https://stackoverflow.com/questions/tagged/dbt?tab=Votes) for snapshots, [`snowflake-cloud-data-platform`](https://stackoverflow.com/questions/tagged/snowflake-cloud-data-platform?tab=Votes) for streams and MERGE

## Canonical check

- [dbt snapshots](https://docs.getdbt.com/docs/build/snapshots) — `hard_deletes` defaults to `ignore`. Prefer `strategy: timestamp` when `updated_at` is reliable, because `check_cols` has to change when the source adds or drops columns.
- [Kimball dimensional modeling techniques](https://www.kimballgroup.com/data-warehouse-business-intelligence-resources/kimball-techniques/dimensional-modeling-techniques/) for slowly changing dimensions
- [Microsoft data-tier guidance](https://microsoft.github.io/code-with-engineering-playbook/design/design-patterns/data-heavy-design-guidance/) — bronze stays a copy of the source so a bad transform can be replayed

Do not add a CDC system because a thread named one. Match the mechanism to the source the project already has.
