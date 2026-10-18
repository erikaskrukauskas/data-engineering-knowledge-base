# Backfill bounds

Historical runs are created with `airflow backfill create` and an explicit date range.

## Look for

- A catchup flag flipped on to reload one week.
- A backfill with no `--max-active-runs`.
- A manual run triggered from the UI for yesterday that does not pass the data interval the SQL expects.

## Fix shape

- Use `--dag-id`, `--from-date`, `--to-date`, and `--max-active-runs`.
- Confirm the task templates read `data_interval_start` and `data_interval_end`.

## Field reports

No thread in the catalog communities stated this failure with a stack and a fix that matches the docs. Search [r/dataengineering](https://www.reddit.com/r/dataengineering/) and the tool tags in `sources/catalog.yaml` before treating a comment as a rule.

## Canonical check

- [Dag runs](https://airflow.apache.org/docs/apache-airflow/stable/core-concepts/dag-run.html) — Backfill is the command that creates runs for a past range. Catchup is the scheduler behavior for missed intervals since `start_date`.

A backfill run is still a DAG run. `max_active_runs` on the DAG applies unless the command overrides it.
