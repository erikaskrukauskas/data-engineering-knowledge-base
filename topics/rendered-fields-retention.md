# Rendered field retention

`[core] num_dag_runs_to_retain_rendered_fields` matches how far back operators need the Rendered tab.

## Look for

- The default 30 on a DAG that is inspected months later.
- The value set very low and then a 500 error on the Rendered tab for an older task.
- A belief that the task log still contains the rendered field after the row is deleted.

## Fix shape

- Raise the count for DAGs whose rendered SQL is part of the audit.
- Accept the missing tab for older runs when the count is kept small to limit the metadata database.

## Field reports

No thread in the catalog communities stated this failure with a stack and a fix that matches the docs. Search [r/dataengineering](https://www.reddit.com/r/dataengineering/) and the tool tags in `sources/catalog.yaml` before treating a comment as a rule.

## Canonical check

- [Configuration reference](https://airflow.apache.org/docs/apache-airflow/stable/configurations-ref.html) — The default is 30 recent DAG runs. Older rendered fields are deleted during task execution, and the Rendered tab can error for those tasks.

This setting was added in Airflow 3.2. It does not exist to tune on 2.x.
