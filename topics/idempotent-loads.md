# Idempotent loads

A rerun of the same interval leaves the target unchanged, aside from replacing that interval's rows.

## Look for

- `INSERT` on a path the orchestrator can retry.
- `datetime.now()`, `current_timestamp()` used as the load window, or a read of "the latest" data inside task logic.
- An incremental model with no `unique_key` and no partition overwrite.
- A raw layer that is truncated and reloaded from a source that can be queried twice with different results.

## Fix shape

- Append the extract to raw storage. Rebuild the downstream table with a merge, a delete-and-insert for that interval, or a partition overwrite.
- Read and write one explicit partition. In Airflow that partition is `data_interval_start`, not the clock at execution time.
- Prefer `MERGE` or upsert over a bare insert when the target already holds the key.

Patterns people report: insert-only raw plus a dbt rebuild; delete the interval in a pre-hook and then insert; partition overwrite when the table is partitioned by the business date.

## Field reports

- [How do you design idempotent data pipelines?](https://www.reddit.com/r/dataengineering/comments/1stehve/how_do_you_design_idempotent_data_pipelines_in/) — r/dataengineering
- Tag page: [`airflow` sorted by votes](https://stackoverflow.com/questions/tagged/airflow?tab=Votes), questions about retries and duplicate inserts

## Canonical check

[Airflow best practices](https://airflow.apache.org/docs/apache-airflow/stable/best-practices.html) — treat a task like a transaction. An `INSERT` on retry duplicates rows. Do not use `now()` for the partition the task reads or writes.

Change a project only when this page and the code agree. The thread shows shapes that have worked; it does not pick the merge key.
