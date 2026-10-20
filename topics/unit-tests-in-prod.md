# Unit tests in production

Unit tests run in development and CI. Production excludes them.

## Look for

- A production `dbt build` with no `--exclude-resource-type unit_test`.
- `DBT_ENGINE_EXCLUDE_RESOURCE_TYPES` unset on the production job.
- A unit test failure blocking a production run of unchanged SQL.

## Fix shape

- Exclude unit tests from the production command.
- Run `dbt test --select test_type:unit` in CI.

## Field reports

No thread in the catalog communities stated this failure with a stack and a fix that matches the docs. Search [r/dataengineering](https://www.reddit.com/r/dataengineering/) and the tool tags in `sources/catalog.yaml` before treating a comment as a rule.

## Canonical check

- [Unit tests](https://docs.getdbt.com/docs/build/unit-tests) — dbt recommends unit tests only in development or CI. Exclude them from production with `--exclude-resource-type` or, on v1.11 and later, `DBT_ENGINE_EXCLUDE_RESOURCE_TYPES`.

Unit tests have existed since dbt v1.8. The environment variable is called out for v1.11 and later. Check the version before relying on the variable name.
