# Airflow and dbt

Airflow loads raw data and starts the transform. dbt owns the SQL from that raw layer through the models consumers query. Both sides take the same date parameters, including on a backfill.

## Look for

- Warehouse SQL copied into Python operators.
- One DAG that ingests sources and rebuilds every mart, with no window parameter.
- A backfill that runs `dbt build --full-refresh` because the model cannot accept a start and end date.
- dbt installed into the Airflow image with pinned versions that conflict (`dbt-core`, `dbt-adapters`, `astronomer-cosmos`, protobuf).
- A backfill DAG that is never scheduled and drifts from the daily DAG.

## Fix shape

- A bronze load task, then a dbt invocation for the silver and gold selectors or tags.
- Pass the Airflow data interval into dbt as variables. The daily run and the backfill set those variables and call the same models.
- When another team owns ingestion, trigger the dbt DAG with a sensor or dataset event instead of one DAG that spans both codebases.
- Pin Cosmos, dbt, and the warehouse adapter together. If the Airflow constraints conflict, run dbt from its own virtualenv or image and point Cosmos at that executable.

Cosmos can render each dbt model as a task. That is optional. `dbt build` as one task is enough when the project does not need per-model retries inside Airflow.

## Field reports

- [How to use Airflow and dbt together?](https://www.reddit.com/r/dataengineering/comments/1k83uee/how_to_use_airflow_and_dbt_together_in_a/) — r/dataengineering. Airflow calls the bronze load, then dbt builds the later layers. Backfill has to be parameterized in both places.
- [Trigger dbt Core jobs for Snowflake using Airflow](https://stackoverflow.com/questions/79744735/trigger-dbt-core-jobs-for-snowflake-using-airflow) — version conflict between Cosmos, dbt-core, and the Snowflake adapter, plus an unknown CLI flag from a mismatched dbt version. This is a dependency note, not a modeling rule.

## Canonical check

- [How dbt structures a project](https://docs.getdbt.com/best-practices/how-we-structure/1-guide-overview) — staging, intermediate, and marts
- [dbt incremental models](https://docs.getdbt.com/docs/build/incremental-models)
- [Airflow best practices](https://airflow.apache.org/docs/apache-airflow/stable/best-practices.html)
