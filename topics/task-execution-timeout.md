# Task execution timeout

Tasks that wait on an API, a warehouse, or a sensor set `execution_timeout`. The default is no timeout.

## Look for

- A Python operator with a `requests` call and no timeout on the call or the task.
- A sensor in poke mode that holds a worker slot until the DAG run times out.
- `execution_timeout` shorter than the warehouse query it launches, so Airflow fails the task while the query keeps running.

## Fix shape

- Set `execution_timeout` above the warehouse statement timeout and below the `dagrun_timeout`.
- Cancel or identify the warehouse query when the task times out, or the next retry will overlap it.

## Field reports

No thread in the catalog communities stated this failure with a stack and a fix that matches the docs. Search [r/dataengineering](https://www.reddit.com/r/dataengineering/) and the tool tags in `sources/catalog.yaml` before treating a comment as a rule.

## Canonical check

- [Configuration reference](https://airflow.apache.org/docs/apache-airflow/stable/configurations-ref.html) — `default_task_execution_timeout` is empty. Operators are not timed out unless a value is set.

A task timeout marks the Airflow task failed. It does not automatically cancel the query the task already submitted.
