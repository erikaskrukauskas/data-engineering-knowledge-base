# Schema contracts and data tests

A model other work depends on declares its columns and types, and a test fails the run when the grain, a required value, or an allowed set breaks. A stale source is visible before downstream models treat silence as an empty success.

## Look for

- A model with no `unique` and `not_null` test on its grain. Older YAML uses `tests:`; current YAML uses `data_tests:`.
- A mart or shared model with no `contract.enforced: true`, or a contract that does not name every column and `data_type`.
- A `unique` or `primary_key` constraint used instead of a data test on Snowflake, BigQuery, Redshift, Databricks, or Spark. Those platforms do not enforce uniqueness at build time. Spark does not enforce `not_null` at build time either. Athena cannot define these constraints.
- An incremental model whose `on_schema_change` is omitted or `ignore`, so a new column never lands. Contracted incremental models need `append_new_columns` or `fail`.
- A source with no `freshness` block. Without `warn_after` or `error_after`, dbt does not check it. `dbt build` does not run freshness.
- A check that rejects rows before they are stored as the raw landing copy, or a raw-to-cleaned step with nowhere to put rejected rows.

## Fix shape

- Test the primary key of every model with `unique` and `not_null`. Test assumptions the source system can fix, such as `accepted_values` and `relationships`, on the source.
- Enforce a contract on public models materialized as `table`, `view`, or `incremental`: every column's `name` and `data_type`. Views record names and types, not constraints. Keep a data test for uniqueness even when the contract lists `not_null`.
- Set `loaded_at_field` or `loaded_at_query`, plus `warn_after` or `error_after`. Run `dbt freshness` where a stale source should stop the job. `dbt source freshness` remains as a legacy command.
- Validate schema and invariants between the raw landing and the cleaned table. Write failures to a malformed store. Leave the raw copy in place so the pipeline can be replayed.

## Field reports

- [So you're using dbt tests — what's next in data quality?](https://www.reddit.com/r/dataengineering/comments/vdp7wl/so_youre_using_dbt_testswhats_next_in_data_quality/) — r/dataengineering. A practitioner comment: downstream tests find ingestion problems after the fact. Define the interface (fields, types, null constraints) for custom integrations, and reconcile source row counts into the raw copy.
- Tag page: [`dbt` sorted by votes](https://stackoverflow.com/questions/tagged/dbt?tab=Votes). That tag already points at incremental loads. Search it for `data_tests`, `contract`, and `freshness`.

## Canonical check

- [dbt data tests](https://docs.getdbt.com/docs/build/data-tests) — `unique`, `not_null`, `accepted_values`, and `relationships`. A test selects the failing rows. `arguments:` holds test inputs on v1.10.5 and later; older files put those inputs at the top level.
- [Tests dbt recommends](https://docs.getdbt.com/faqs/Tests/recommended-tests) — every model has `unique` and `not_null` on a primary key.
- [Model contracts](https://docs.getdbt.com/docs/mesh/govern/model-contracts) — the build fails when the query's column names and types do not match the contract. Contracts do not apply to Python models, ephemeral models, sources, seeds, or snapshots. Most warehouses enforce only `not_null`.
- [Freshness config](https://docs.getdbt.com/reference/resource-configs/freshness) — one of `warn_after` or `error_after` is required, each with `count` and `period`.
- [Microsoft data-tier guidance](https://microsoft.github.io/code-with-engineering-playbook/design/design-patterns/data-heavy-design-guidance/) — validate between bronze and silver, and quarantine rows that fail. Do not drop the bronze copy to perform that check.

Do not add a contract to every staging model that is still changing shape. dbt recommends contracts for public models other teams or tools query.
