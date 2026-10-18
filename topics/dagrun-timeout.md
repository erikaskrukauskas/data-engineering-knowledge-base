# Dag run timeout

A DAG that can hang sets `dagrun_timeout` so the run fails and the next interval can be created.

## Look for

- A DAG with `max_active_runs` of 1 and no `dagrun_timeout`, so one hung run blocks every later interval.
- A timeout shorter than the slowest successful run, so healthy backfills fail.
- A task `execution_timeout` used as a substitute for the run timeout. They measure different things.

## Fix shape

- Set `dagrun_timeout` to longer than a normal run and shorter than the gap you can tolerate before the next interval starts.
- Set `execution_timeout` on the task that calls an external system.

## Field reports

No thread in the catalog communities stated this failure with a stack and a fix that matches the docs. Search [r/dataengineering](https://www.reddit.com/r/dataengineering/) and the tool tags in `sources/catalog.yaml` before treating a comment as a rule.

## Canonical check

- [DAG model](https://airflow.apache.org/docs/apache-airflow/stable/_api/airflow/models/dag/index.html) — `dagrun_timeout` is how long a DagRun may stay up before it fails, so later DagRuns can be created. It defaults to none.

The core `default_task_execution_timeout` is empty, which means operators are not timed out unless you set one. A DAG timeout does not fill that in.
