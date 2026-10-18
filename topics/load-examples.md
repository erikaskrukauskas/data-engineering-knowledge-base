# Example DAGs

`load_examples` is false on a production environment.

## Look for

- `AIRFLOW__CORE__LOAD_EXAMPLES` left at the core default of true outside the official Docker image.
- Example DAGs paused but still parsed on every scheduler loop.
- A production deploy that copied a dev compose file with examples enabled.

## Fix shape

- Set `AIRFLOW__CORE__LOAD_EXAMPLES=False` in production.
- The official Docker image already defaults this environment variable to false. Set it back to true only when you mean to.

## Field reports

No thread in the catalog communities stated this failure with a stack and a fix that matches the docs. Search [r/dataengineering](https://www.reddit.com/r/dataengineering/) and the tool tags in `sources/catalog.yaml` before treating a comment as a rule.

## Canonical check

- [Configuration reference](https://airflow.apache.org/docs/apache-airflow/stable/configurations-ref.html) — The core default is true. The official image sets the environment variable to false. Turn examples on there only by setting it to true.

Turning the flag off does not delete example DAG rows already stored in the metadata database. Remove those DAGs if they still appear.
