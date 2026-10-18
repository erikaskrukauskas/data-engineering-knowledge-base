# Metadata database backup

The metadata database is part of the scheduler. It is backed up, and old rows are cleaned, on a schedule.

## Look for

- No backup of the database that stores DAG runs and task state.
- No cleanup, so the database grows until the scheduler slows.
- A cleanup that deletes current runs or the last rendered fields you still need.

## Fix shape

- Back up the metadata database with the same care as the warehouse.
- Prune old rows on a schedule. Keep rendered fields for the runs operators still open.

## Field reports

No thread in the catalog communities stated this failure with a stack and a fix that matches the docs. Search [r/dataengineering](https://www.reddit.com/r/dataengineering/) and the tool tags in `sources/catalog.yaml` before treating a comment as a rule.

## Canonical check

- [Airflow best practices](https://airflow.apache.org/docs/apache-airflow/stable/best-practices.html) — The metadata database has to be backed up and its size managed. Scheduler health depends on it.

`num_dag_runs_to_retain_rendered_fields` is one cleanup. It is not a backup, and it defaults to 30 runs.
