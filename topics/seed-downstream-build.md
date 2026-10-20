# Seed downstream builds

Models that `ref` a seed are built with the seed name plus the downstream selector. `dbt seed` loads the CSV. Selection treats the seed like a model.

## Look for

- A changed CSV and a job that runs `dbt build --select state:modified+` without the seed resource included.
- `dbt run --select country_codes` and surprise that the seed command was the one that loads rows.
- Downstream models reading a truncated seed because `dbt seed` ran and the models did not.

## Fix shape

- Run `dbt seed --select <seed>` when the CSV changes.
- Run `dbt run --select <seed>+` to rebuild consumers.

## Field reports

No thread in the catalog communities stated this failure with a stack and a fix that matches the docs. Search [r/dataengineering](https://www.reddit.com/r/dataengineering/) and the tool tags in `sources/catalog.yaml` before treating a comment as a rule.

## Canonical check

- [Seeds](https://docs.getdbt.com/docs/build/seeds) — `dbt seed` loads the file. Downstream models use `ref`. `dbt run --select <seed>+` runs models downstream of the seed.

`dbt seed` is its own command. A job that only runs `dbt run` does not reload the CSV.
