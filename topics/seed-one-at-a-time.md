# Selecting one seed

`dbt seed --select` loads the named seed. `--exclude` skips one. The default command loads every seed.

## Look for

- `dbt seed` in CI after one CSV edit, reloading every mapping table.
- A production job that seeds all files on every run.
- `--select` used with `dbt run` and expected to reload the CSV.

## Fix shape

- Select the seed on the seed command.
- Exclude seeds that did not change.

## Field reports

No thread in the catalog communities stated this failure with a stack and a fix that matches the docs. Search [r/dataengineering](https://www.reddit.com/r/dataengineering/) and the tool tags in `sources/catalog.yaml` before treating a comment as a rule.

## Canonical check

- [Seeds](https://docs.getdbt.com/docs/build/seeds) — `dbt seed --select` and `--exclude` limit which seed files load. Selection syntax is the same idea as model selection.

Selecting a seed on `dbt run` does not load the CSV. The seed command does.
