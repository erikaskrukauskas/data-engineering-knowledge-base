# Max active tasks

`max_active_tasks` caps how many task instances of one DAG run execute at once. The default is 16.

## Look for

- A mapped task expanded to hundreds of tasks with `max_active_tasks` left at 16, or raised to the scheduler `parallelism` so one DAG fills the cluster.
- An old DAG that still sets `concurrency`, which is the older name, to a value that surprises the newer `max_active_tasks`.
- A backfill DAG whose early start date is allowed to occupy every slot.

## Fix shape

- Set `max_active_tasks` to the number of concurrent warehouse or API calls one run may make.
- Keep it below `parallelism` so other DAGs can run.

## Field reports

No thread in the catalog communities stated this failure with a stack and a fix that matches the docs. Search [r/dataengineering](https://www.reddit.com/r/dataengineering/) and the tool tags in `sources/catalog.yaml` before treating a comment as a rule.

## Canonical check

- [Configuration reference](https://airflow.apache.org/docs/apache-airflow/stable/configurations-ref.html) — `max_active_tasks_per_dag` defaults to 16. The config text names the case: a DAG run with an early start date should not steal every executor slot.

This cap is per DAG run. `max_active_runs` is per DAG. A catchup with both left high is the combination that fills the cluster.
