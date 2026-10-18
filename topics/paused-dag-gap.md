# Paused DAG gap

With catchup off, turning a DAG back on continues from the latest interval. The paused days are not filled in.

## Look for

- A DAG paused for a week and then unpaused, with the assumption that seven daily runs will appear.
- `catchup=True` left on so the unpause does replay, and the tasks use `now()`.
- No backfill command after an intentional pause that should have been loaded.

## Fix shape

- Leave catchup off. Run `airflow backfill create` with the paused dates when those dates must land.
- Point the backfill at the data interval, not the wall clock.

## Field reports

No thread in the catalog communities stated this failure with a stack and a fix that matches the docs. Search [r/dataengineering](https://www.reddit.com/r/dataengineering/) and the tool tags in `sources/catalog.yaml` before treating a comment as a rule.

## Canonical check

- [Timetables](https://airflow.apache.org/docs/apache-airflow/stable/authoring-and-scheduling/timetable.html) — A paused DAG that is turned back on does not run the intervals from the paused period when catchup is false.

If catchup is true, the unpause does create those runs. Check the DAG argument and `catchup_by_default` before pausing a production DAG.
