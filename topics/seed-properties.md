# Seed tests

Seed properties live under a `seeds:` key and can carry `data_tests` and descriptions.

## Look for

- A country-code seed with no unique test on the code.
- Seed tests declared under `models:`.
- A seed documented only in a comment in the CSV.

## Fix shape

- Add `seeds:` YAML with the description and column tests.
- Test the key the downstream models join on.

## Field reports

No thread in the catalog communities stated this failure with a stack and a fix that matches the docs. Search [r/dataengineering](https://www.reddit.com/r/dataengineering/) and the tool tags in `sources/catalog.yaml` before treating a comment as a rule.

## Canonical check

- [Seeds](https://docs.getdbt.com/docs/build/seeds) — Document and test seeds in YAML under `seeds:`. The example puts `unique` and `not_null` on the code and the name.

Seed properties do not set `column_types`. Types stay in `dbt_project.yml` under the project name.
