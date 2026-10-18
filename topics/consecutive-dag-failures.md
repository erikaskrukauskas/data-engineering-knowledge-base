# Consecutive DAG failures

`max_consecutive_failed_dag_runs` stays 0 unless someone decided the DAG should pause itself.

## Look for

- A value copied from an example so a flaky source pauses the DAG after one night.
- The setting left at 0 on a DAG that pages people and then keeps creating failed runs all weekend.
- A paused DAG that nobody unpauses after the source recovers.

## Fix shape

- Leave the default 0 when a human should decide to pause.
- Set a small integer only on a DAG whose repeated failure is itself the incident, and document who unpauses it.

## Field reports

No thread in the catalog communities stated this failure with a stack and a fix that matches the docs. Search [r/dataengineering](https://www.reddit.com/r/dataengineering/) and the tool tags in `sources/catalog.yaml` before treating a comment as a rule.

## Canonical check

- [Configuration reference](https://airflow.apache.org/docs/apache-airflow/stable/configurations-ref.html) — `[core] max_consecutive_failed_dag_runs_per_dag` defaults to 0, so DAGs are not auto-paused. The DAG argument uses that value. The setting is marked experimental.

This is experimental as of the 3.3.2 configuration page. Do not treat a pause as a guaranteed alert. Pair it with a notification.
