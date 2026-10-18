# Max active runs

The scheduler stops creating runs when `max_active_runs` running instances already exist.

## Look for

- A backfill or catchup DAG that leaves `max_active_runs` at the environment default of 16 while each run is heavy.
- A sensor DAG allowed 16 concurrent runs that all poke the same external system.
- `max_consecutive_failed_dag_runs` left unset on a DAG that should disable itself after repeated failures.

## Fix shape

- Set `max_active_runs` on the DAG to the number of intervals the warehouse can rebuild at once.
- Pass `--max-active-runs` on `airflow backfill create`.
- Set `dagrun_timeout` so a stuck run fails and frees a slot.

## Field reports

No thread in the catalog communities stated this failure with a stack and a fix that matches the docs. Search [r/dataengineering](https://www.reddit.com/r/dataengineering/) and the tool tags in `sources/catalog.yaml` before treating a comment as a rule.

## Canonical check

- [DAG model](https://airflow.apache.org/docs/apache-airflow/stable/_api/airflow/models/dag/index.html) — `max_active_runs` is the cap on DAG runs in a running state. Past that cap the scheduler does not create another active run. The default comes from `[core] max_active_runs_per_dag`, which is 16.
- [Configuration reference](https://airflow.apache.org/docs/apache-airflow/stable/configurations-ref.html) — The scheduler will not create more DAG runs after the per-DAG limit.

`max_active_tasks` is a different cap, on task instances inside the DAG. Raising one does not raise the other.
