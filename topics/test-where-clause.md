# Test where clause

`where` on a test limits the rows that are checked. It is not a filter to make a failing test pass.

## Look for

- `where: "updated_at > current_date"` on a `unique` test so old duplicates are ignored.
- A `not_null` test with a where clause that excludes the nulls.
- A config `where` in `dbt_project.yml` applied to every test of that type.

## Fix shape

- Use `where` for a subset that is the real grain, such as the current partition, and state that in the test description.
- Fix or quarantine the old duplicates instead of filtering them out of the only uniqueness test.

## Field reports

No thread in the catalog communities stated this failure with a stack and a fix that matches the docs. Search [r/dataengineering](https://www.reddit.com/r/dataengineering/) and the tool tags in `sources/catalog.yaml` before treating a comment as a rule.

## Canonical check

- [dbt data tests](https://docs.getdbt.com/docs/build/data-tests) — Test configs, including `where`, change which rows the test query sees. The test fails when that query returns rows.

A where clause that references `current_date` is not idempotent. The test result changes with the clock.
