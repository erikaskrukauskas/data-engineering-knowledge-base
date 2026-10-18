# Failure alerts

A failed task notifies someone. A run that never finishes notifies someone before the DAG is marked done.

## Look for

- A DAG with no `on_failure_callback`, no notifier, and `email_on_failure` left false.
- A callback that only logs, or that sends only when an environment variable says the run is prod, with no equivalent check on the production deploy.
- `sla` / `sla_miss_callback` used as the only late-run alarm. Airflow evaluates that callback when the DAG run finishes, so a run that never finishes never pages.
- An alert path that depends on the worker finishing the task. A task the scheduler marks failed, or a worker that disappears, does not invoke `on_failure_callback`.
- Callback errors investigated only in the task log. Callback failures are written to the DAG processor log.

## Fix shape

- Set `on_failure_callback` on the DAG, or in `default_args`, to a notifier that reaches a channel a person watches. Pass a list when more than one notifier is required.
- For a runtime limit, add a Deadline Alert (Airflow 3.1 and later) with a reference, an interval, and a callback. The scheduler fires it when the deadline passes, including while the DAG is still running.
- Keep a check outside Airflow for the scheduler and the workers. A DAG cannot page you when the scheduler is down.

## Field reports

- [How do you alert for Airflow errors?](https://www.reddit.com/r/dataengineering/comments/zf40rw/how_do_you_like_to_alert_for_airflow_errors/) — r/dataengineering. `on_failure_callback` never ran when the task was not started or the worker pod was evicted. The docs agree that a worker must change the state for that callback to run, so a deadline or an external check has to cover the other cases.

## Canonical check

- [Airflow callbacks](https://airflow.apache.org/docs/apache-airflow/stable/administration-and-deployment/logging-monitoring/callbacks.html) — `on_failure_callback` runs when a worker marks the DAG or task failed. CLI and UI state changes do not run it. Skipped tasks that never started do not run `on_skipped_callback`.
- [Deadline Alerts](https://airflow.apache.org/docs/apache-airflow/stable/howto/deadline-alerts.html) — a reference, an interval, and a callback. This feature is marked experimental as of Airflow 3.1.
- [Migrating from SLA to Deadline Alerts](https://airflow.apache.org/docs/apache-airflow/stable/howto/sla-to-deadlines.html) — an SLA miss callback waits until the DAG run finishes.

Do not apply Deadline Alerts to an Airflow 2 deploy. Use `sla_miss_callback` there and accept that it runs only after the DAG finishes, plus an external check for runs that hang.
