# SLA miss timing

An SLA miss callback runs in the context the SLA page describes, which is not a substitute for a task failure callback.

## Look for

- `sla` set on a task and no `on_failure_callback`, because the SLA was expected to page on failure.
- An SLA shorter than the schedule interval.
- A migration to Deadline Alerts that keeps the old SLA numbers without reading the migration page.

## Fix shape

- Page on task failure with the failure callback.
- Use an SLA or a deadline only for lateness, and read the migration page before moving the numbers.

## Field reports

No thread in the catalog communities stated this failure with a stack and a fix that matches the docs. Search [r/dataengineering](https://www.reddit.com/r/dataengineering/) and the tool tags in `sources/catalog.yaml` before treating a comment as a rule.

## Canonical check

- [SLA migration](https://airflow.apache.org/docs/apache-airflow/stable/howto/sla-to-deadlines.html) — The SLA mechanism is being replaced by deadlines. Copying the old parameter forward is not the migration.
- [Airflow callbacks](https://airflow.apache.org/docs/apache-airflow/stable/administration-and-deployment/logging-monitoring/callbacks.html) — Failure callbacks are the path for a task that failed. They are a different hook from an SLA miss.

The failure-alerts note covers a callback that never runs because no worker started. This note covers using an SLA as that callback.
