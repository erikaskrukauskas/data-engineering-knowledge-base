# Test severity

Primary-key tests error. Warn is for a check that must not stop the build.

## Look for

- `severity: warn` on `unique` or `not_null` of a mart grain.
- A package of project-evaluator checks left at warn in CI, so the job is green while the checks fail.
- Tests that are not selected. `dbt run` does not run tests. `dbt build` does.

## Fix shape

- Leave grain tests at error.
- Set the evaluator severity to error in CI and warn in development.
- Use `dbt build` in the deploy job.

## Field reports

No thread in the catalog communities stated this failure with a stack and a fix that matches the docs. Search [r/dataengineering](https://www.reddit.com/r/dataengineering/) and the tool tags in `sources/catalog.yaml` before treating a comment as a rule.

## Canonical check

- [dbt data tests](https://docs.getdbt.com/docs/build/data-tests) — A test fails when it returns rows. Severity is a config. `dbt test` and `dbt build` run tests. `dbt run` does not.
- [dbt CI](https://docs.getdbt.com/guides/set-up-ci) — Project-evaluator tests default to warn. Set an environment variable so CI treats them as errors.

`store_failures` keeps the failing rows and does not change severity. A warn with stored failures is still a green job.
