# Scheduler parallelism

`[core] parallelism` bounds running task instances per scheduler. Adding workers does not raise it.

## Look for

- Workers scaled out while tasks stay queued and `parallelism` is still 32.
- More than one scheduler with `parallelism` set as if it were a cluster-wide cap. The docs say the cap is per scheduler.
- `parallelism` set to 1 during a test and left there.

## Fix shape

- Size `parallelism` for the metadata database and the workers together.
- Remember the cluster maximum is that value times the number of schedulers.

## Field reports

No thread in the catalog communities stated this failure with a stack and a fix that matches the docs. Search [r/dataengineering](https://www.reddit.com/r/dataengineering/) and the tool tags in `sources/catalog.yaml` before treating a comment as a rule.

## Canonical check

- [Configuration reference](https://airflow.apache.org/docs/apache-airflow/stable/configurations-ref.html) — `parallelism` is the maximum number of task instances that can run concurrently per scheduler, default 32. The value must be at least 1.

`max_active_tasks_per_dag` and pool slots can queue tasks before this cap is reached. Check all three when a task stays queued.
