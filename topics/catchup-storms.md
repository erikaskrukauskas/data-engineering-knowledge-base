# Catchup storms

Turning a DAG on creates a run for the latest interval only, unless someone asked for history.

## Look for

- `catchup=True` on a DAG whose tasks use the clock instead of the data interval.
- A `start_date` months in the past on a DAG that was just deployed.
- `[scheduler] catchup_by_default` set back to true for the whole environment.

## Fix shape

- Leave `catchup` false. Create history with `airflow backfill create` and an explicit `--from-date`, `--to-date`, and `--max-active-runs`.
- Pass the data interval into the task. A catchup run must write that interval, not whatever is true at execution time.

## Field reports

- [Why does Airflow keep running?](https://www.reddit.com/r/dataengineering/comments/lyu1k5/why_does_apache_airflow_keep_running/) — r/dataengineering. A past `start_date` with catchup enabled launched a run for every missed window, seconds apart. The docs' fix is catchup off, plus an explicit backfill when history is required.

## Canonical check

- [Dag runs](https://airflow.apache.org/docs/apache-airflow/stable/core-concepts/dag-run.html) — With `catchup=True`, the scheduler creates a run for every completed interval since `start_date`. The 3.3.2 default is off, via `scheduler.catchup_by_default=False`.
- [Timetables](https://airflow.apache.org/docs/apache-airflow/stable/authoring-and-scheduling/timetable.html) — A paused DAG that is turned back on does not replay the paused period when catchup is false.

The DAG class docstring still says catchup defaults to true. Trust the 3.3.2 Dag Runs page and `catchup_by_default`, and read the value in the running config before changing a fleet of DAGs.
