# Uniqueness not enforced

`unique` is a dbt test. Most warehouses do not enforce uniqueness when the model builds.

## Look for

- A contract `constraints` entry of `unique` treated as a Snowflake or BigQuery primary key.
- No `unique` test, because the contract was expected to reject a duplicate.
- A Spark model with `not_null` in the contract and no test. Spark does not enforce `not_null` at build.

## Fix shape

- Keep `unique` and `not_null` tests on the grain.
- Read the contract page for the adapter before telling a stakeholder the warehouse rejected a duplicate.

## Field reports

No thread in the catalog communities stated this failure with a stack and a fix that matches the docs. Search [r/dataengineering](https://www.reddit.com/r/dataengineering/) and the tool tags in `sources/catalog.yaml` before treating a comment as a rule.

## Canonical check

- [Model contracts](https://docs.getdbt.com/docs/mesh/govern/model-contracts) — Most adapters enforce only `not_null`. Snowflake, BigQuery, Redshift, Databricks, and Spark do not enforce uniqueness at build. Spark does not enforce `not_null` at build. Athena cannot define those constraints.

A test fails the dbt job after the relation is built. It does not roll back a warehouse that already committed the table, unless the materialization is inside a transaction that includes the tests. Do not assume that. The test has to fail the job you gate on.
