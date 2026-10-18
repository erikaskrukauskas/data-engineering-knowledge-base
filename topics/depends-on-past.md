# Depends on past

A task waits for the previous interval only when that wait is the business rule.

## Look for

- `depends_on_past=True` in `default_args` for a DAG that loads independent days.
- A single old failed task instance that leaves every later run queued.
- The first-ever run assumed to wait. Airflow still runs it, because there is no previous instance.

## Fix shape

- Set `depends_on_past` only on the task that must not start until yesterday succeeded.
- Clear or mark the blocking instance when the data for that interval is already repaired.

## Field reports

No thread in the catalog communities stated this failure with a stack and a fix that matches the docs. Search [r/dataengineering](https://www.reddit.com/r/dataengineering/) and the tool tags in `sources/catalog.yaml` before treating a comment as a rule.

## Canonical check

- [DAGs](https://airflow.apache.org/docs/apache-airflow/stable/core-concepts/dags.html) — A task with `depends_on_past=True` runs only after the previous scheduled instance succeeded or was skipped. The first automated run has nothing to wait for.

An operator argument overrides the same key in `default_args`. A `False` on the operator wins over a `True` in `default_args`.
