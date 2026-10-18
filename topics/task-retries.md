# Task retries

Retries are set on the task. The environment default is zero.

## Look for

- `retries` in `default_args` for a DAG that mixes an idempotent load with a non-idempotent export.
- A retry delay left at 300 seconds on a task that hits a rate limit and needs longer.
- A task with retries whose SQL inserts without a key.

## Fix shape

- Set `retries` on the idempotent tasks. Leave it at 0 on a task that must not run twice.
- Keep the partition or merge key so a retry replaces the same rows.

## Field reports

No thread in the catalog communities stated this failure with a stack and a fix that matches the docs. Search [r/dataengineering](https://www.reddit.com/r/dataengineering/) and the tool tags in `sources/catalog.yaml` before treating a comment as a rule.

## Canonical check

- [Configuration reference](https://airflow.apache.org/docs/apache-airflow/stable/configurations-ref.html) — `default_task_retries` is 0. `default_task_retry_delay` is 300 seconds. Both can be overridden on the DAG or the task.
- [Idempotence in dbt](https://docs.getdbt.com/best-practices/idempotence) — A rerun has to replace the same result. A retry of an insert-only task is how duplicates appear.

Airflow will retry a failed task even when the failure happened after a partial write. The retry count does not make the write idempotent.
