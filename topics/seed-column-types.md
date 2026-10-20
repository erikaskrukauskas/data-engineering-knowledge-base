# Seed column types

`column_types` forces a varchar when the CSV must keep leading zeros or a code that looks numeric.

## Look for

- A zip or account column loaded as a number, with leading zeros gone.
- No `+column_types` under the project name in `dbt_project.yml`.
- A type set in the seed YAML properties and ignored because `column_types` belongs in the project config.

## Fix shape

- Set `column_types` under `seeds: <project_name>: <seed_name>`.
- Keep the leading zeros in the CSV and type the column as varchar of the right length.

## Field reports

No thread in the catalog communities stated this failure with a stack and a fix that matches the docs. Search [r/dataengineering](https://www.reddit.com/r/dataengineering/) and the tool tags in `sources/catalog.yaml` before treating a comment as a rule.

## Canonical check

- [Seeds](https://docs.getdbt.com/docs/build/seeds) — dbt infers types from the CSV. `column_types` overrides that. Leading zeros survive only when the column is a varchar.

The project name is required in that YAML path. A `column_types` block at the wrong indent does not apply.
