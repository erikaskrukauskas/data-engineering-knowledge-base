# Required environment variables

`env_var` reads the process environment. A missing variable fails unless a default was passed.

## Look for

- `env_var('DB_PASSWORD')` with no default in a model that only some jobs can render.
- A default of an empty string, so a missing secret becomes a blank password.
- A secret concatenated or cast in Jinja inside a `DBT_ENV_SECRET_` value.

## Fix shape

- Call `env_var` with no default for anything that must exist.
- Keep `DBT_ENV_SECRET_` values as a single setting in `profiles.yml` or `packages.yml`, not in model SQL.

## Field reports

No thread in the catalog communities stated this failure with a stack and a fix that matches the docs. Search [r/dataengineering](https://www.reddit.com/r/dataengineering/) and the tool tags in `sources/catalog.yaml` before treating a comment as a rule.

## Canonical check

- [env_var](https://docs.getdbt.com/reference/dbt-jinja-functions/env_var) — The function reads an environment variable and can take a default. A missing variable without a default fails.
- [dbt packages](https://docs.getdbt.com/docs/build/packages) — Jinja in `packages.yml` is how a git token is supplied. `dependencies.yml` does not render it.

A default hides a missing production variable and points the run at the wrong database. Use a default only for a value that is safe in every environment.
