# Alert fan-out

A DAG with many tasks notifies once per run, not once per failed task.

## Look for

- `email_on_failure` or `on_failure_callback` set on every task of a wide DAG.
- A fan-out of dozens of mapped tasks that each send mail.
- No aggregate step that runs when upstream tasks finish, successful or not.

## Fix shape

- Put the notifier on the DAG, or on one collector task that runs after the others with a trigger rule that still fires when some upstream tasks failed.
- Have each task return a small status. Send one message that lists the failed task ids.

## Field reports

- [Email floods from DAG failures](https://www.reddit.com/r/dataengineering/comments/1njibu9/how_to_avoid_email_floods_from_airflow_dag/) — r/dataengineering. Thirty concurrent tasks each sent a failure mail. The fix that matches the callback docs is one collector that reports the failed task ids once.

## Canonical check

- [Airflow callbacks](https://airflow.apache.org/docs/apache-airflow/stable/administration-and-deployment/logging-monitoring/callbacks.html) — Callbacks can be set on the DAG, in `default_args` for every task, or on one task. A DAG-level `on_failure_callback` covers the run.

A DAG callback still does not run when the scheduler marks a task failed without a worker. That case is the failure-alerts topic.
