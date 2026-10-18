# Static start date

`start_date` is a fixed, timezone-aware instant in the past.

## Look for

- `start_date=datetime.now()` or `pendulum.now()` so the interval changes every parse.
- A naive datetime with no timezone.
- A `start_date` equal to today on a daily DAG, so the first run waits until that interval has ended.

## Fix shape

- Set `start_date` with `pendulum.datetime(..., tz="UTC")` to a real past date.
- Let the scheduler create a run only after the interval has ended.

## Field reports

No thread in the catalog communities stated this failure with a stack and a fix that matches the docs. Search [r/dataengineering](https://www.reddit.com/r/dataengineering/) and the tool tags in `sources/catalog.yaml` before treating a comment as a rule.

## Canonical check

- [Dag runs](https://airflow.apache.org/docs/apache-airflow/stable/core-concepts/dag-run.html) — The data interval is a window that has already closed. The examples use a fixed Pendulum datetime in UTC.
- [DAGs](https://airflow.apache.org/docs/apache-airflow/stable/core-concepts/dags.html) — The scheduler creates a run for an interval after that interval ends.

A fixed start date in the past does not by itself replay history. Replay is the catchup setting.
