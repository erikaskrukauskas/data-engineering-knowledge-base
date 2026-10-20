# Seed column changes

Adding or renaming a seed column requires `dbt seed --full-refresh`. A normal seed truncates and reinserts and fails when the table structure changed.

## Look for

- A new CSV column and a `dbt seed` without `--full-refresh`.
- An error such as invalid identifier or column does not exist on the seed table.
- `--full-refresh` used on every run, which drop-cascades the table.

## Fix shape

- Full-refresh when the header changes.
- Leave it off when only row values changed, so downstream objects are not dropped.

## Field reports

No thread in the catalog communities stated this failure with a stack and a fix that matches the docs. Search [r/dataengineering](https://www.reddit.com/r/dataengineering/) and the tool tags in `sources/catalog.yaml` before treating a comment as a rule.

## Canonical check

- [Seeds](https://docs.getdbt.com/docs/build/seeds) — Normal seed truncates and reinserts to avoid `drop cascade`. A column change fails until `--full-refresh`, and that flag drops the table with cascade before rebuilding.

Cascade can drop objects built on the seed, including ones a BI tool is querying. Full-refresh is for the schema change, then the downstream models need a rebuild.
