# Deadline alerts

Deadline Alerts exist from Airflow 3.1 and are marked experimental. Older deployments keep a different late-task signal.

## Look for

- A 3.1 Deadline Alert copied into an Airflow 2 DAG.
- An SLA miss callback treated as the deadline feature.
- A deadline callback that assumes it runs before the DAG finishes.

## Fix shape

- On 3.1 and later, define a deadline with a reference, an interval, and a callback, and treat the feature as experimental.
- On Airflow 2, do not invent a deadline block. Use the failure callback path that version has.

## Field reports

No thread in the catalog communities stated this failure with a stack and a fix that matches the docs. Search [r/dataengineering](https://www.reddit.com/r/dataengineering/) and the tool tags in `sources/catalog.yaml` before treating a comment as a rule.

## Canonical check

- [Deadline alerts](https://airflow.apache.org/docs/apache-airflow/stable/howto/deadline-alerts.html) — Deadline Alerts are experimental as of Airflow 3.1. They take a reference, an interval, and a callback.
- [SLA migration](https://airflow.apache.org/docs/apache-airflow/stable/howto/sla-to-deadlines.html) — SLAs are the older mechanism. The migration page is the map onto deadlines, not a promise that SLA syntax still works unchanged.

Confirm the Airflow version in the running image before adding either block. The stable docs describe 3.3.2.
