# Stored test failures

`store_failures` writes the failing rows to the database. The schema has to be one that job can create.

## Look for

- `store_failures: true` in CI with a target schema the CI user cannot create.
- Stored failures in the production schema, queried as if they were a mart.
- Severity left at warn, so the job is green and the failure table is the only signal.

## Fix shape

- Store failures in the job's own schema.
- Keep grain tests at error so the table is a record of a failed job, not a second mart.

## Field reports

No thread in the catalog communities stated this failure with a stack and a fix that matches the docs. Search [r/dataengineering](https://www.reddit.com/r/dataengineering/) and the tool tags in `sources/catalog.yaml` before treating a comment as a rule.

## Canonical check

- [dbt data tests](https://docs.getdbt.com/docs/build/data-tests) — `store_failures` saves the rows the test returned. It does not change whether the test warns or errors.

The failure table is overwritten on the next run of that test. It is not a history of every past failure.
