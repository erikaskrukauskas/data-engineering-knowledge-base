# Template strict undefined

Airflow templates use strict undefined. A missing variable is an error, not an empty string.

## Look for

- A SQL file that references `{{ params.day }}` when the DAG has no such param.
- A template changed to a custom undefined class that renders missing variables as blank, so the SQL runs against the wrong date.
- `execution_date` used in a 3.x DAG where the template the task should read is the data interval.

## Fix shape

- Define the param or use `data_interval_start`.
- Leave `template_undefined` at `StrictUndefined`.

## Field reports

No thread in the catalog communities stated this failure with a stack and a fix that matches the docs. Search [r/dataengineering](https://www.reddit.com/r/dataengineering/) and the tool tags in `sources/catalog.yaml` before treating a comment as a rule.

## Canonical check

- [DAG model](https://airflow.apache.org/docs/apache-airflow/stable/_api/airflow/models/dag/index.html) — The DAG constructor defaults `template_undefined` to `jinja2.StrictUndefined`.
- [Dag runs](https://airflow.apache.org/docs/apache-airflow/stable/core-concepts/dag-run.html) — The interval for a scheduled run is the data interval, available to templates.

A manual run may not have the interval a scheduled run has, unless the trigger passes it. Strict undefined is what makes that mistake visible.
