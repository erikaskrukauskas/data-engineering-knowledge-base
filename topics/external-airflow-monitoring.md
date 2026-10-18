# External Airflow monitoring

A process outside the Airflow scheduler watches that runs finish. Metrics leave the metadata database.

## Look for

- A DAG whose only health check is another DAG in the same Airflow.
- No StatsD, logs backend, or external probe of the scheduler.
- Success messages on every task and no failure signal. The noise hides the outage.

## Fix shape

- Export task and DAG metrics to a system that is not the scheduler.
- Alert on failure, not on every success.
- Probe the scheduler and the metadata database from outside the cluster.

## Field reports

- [DAG monitoring tips](https://www.reddit.com/r/dataengineering/comments/1o2tgtq/need_airflow_dag_monitoring_tips/) — r/dataengineering. Using a DAG to watch other DAGs works until Airflow itself is down. The reply that matches the docs is to push metrics out and alert on failures.

## Canonical check

- [Airflow callbacks](https://airflow.apache.org/docs/apache-airflow/stable/administration-and-deployment/logging-monitoring/callbacks.html) — Callbacks run inside Airflow after a worker changes state. They do not run when the scheduler is down.
- [Airflow best practices](https://airflow.apache.org/docs/apache-airflow/stable/best-practices.html) — The metadata database has to be backed up and pruned. It is part of the system to watch, not only the tasks.

A completion file written by the task can show that a run finished. It cannot show that the scheduler failed to create the run. Pair it with a check that the run existed.
