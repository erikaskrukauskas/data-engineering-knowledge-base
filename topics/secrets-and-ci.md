# Secrets and CI

Credentials live in a secret store, not in the repository or the orchestrator metadata database. A pull request builds only the models it changed, in a schema that is not production.

## Look for

- A password, token, or private key written into `profiles.yml`, a DAG, a notebook, or a committed `.env`.
- `env_var('DBT_ENV_SECRET_...')` inside model SQL or `dbt_project.yml`. dbt allows that prefix only in `profiles.yml` and `packages.yml`.
- An Airflow connection or variable password stored only in the metadata database, with no secrets backend configured.
- A CI job that runs `dbt build` or `dbt run` with no `state:modified+` selector, or that writes into the production schema.
- `--full-refresh` on the pull-request job.
- A production deploy that does not run `dbt build` (models and tests) after merge.

## Fix shape

- Read warehouse passwords with `env_var`. Prefix values that must be scrubbed from logs with `DBT_ENV_SECRET_`. One secret per setting. Do not concatenate or cast a secret with Jinja.
- Point Airflow at a secrets backend. The backend is searched before environment variables and before the metadata database.
- On a pull request, run `dbt build --select state:modified+` against a manifest from the production job, with `--defer` so unchanged parents are read from production. Give each pull request its own schema.
- After merge, deploy the same artifact through a non-production run and then production. Scan commits for credentials.

## Field reports

- [CI/CD process for dbt models](https://www.reddit.com/r/dataengineering/comments/yi5ay3/cicd_process_for_dbt_models/) — r/dataengineering. The failure is a pipeline that only fails at the mart, or a CI job that rebuilds the project. The agreed fix is lint, then `dbt build` of modified models and their children in a dev target, deferring to a production manifest, and deploying production only after that run succeeds.

## Canonical check

- [dbt environment variables](https://docs.getdbt.com/reference/dbt-jinja-functions/env_var) — `DBT_ENV_SECRET_` values are scrubbed from logs and cannot appear in model SQL.
- [dbt CI](https://docs.getdbt.com/guides/set-up-ci) — CI builds `state:modified+` in its own schema. Compare state to the production job, not to a busy staging environment whose manifest changes under open pull requests.
- [Defer](https://docs.getdbt.com/reference/node-selection/defer) — unselected parents resolve from the state manifest when they are absent in the CI database. Ephemeral models are never deferred.
- [Airflow secrets backend](https://airflow.apache.org/docs/apache-airflow/stable/security/secrets/secrets-backend/index.html) — set `[secrets] backend`. A duplicate key is written to the metadata database and read from the backend. The UI does not show backend values.
- [Microsoft data-tier guidance](https://microsoft.github.io/code-with-engineering-playbook/design/design-patterns/data-heavy-design-guidance/) — secrets in a vault per environment, and CI that includes credential scanning.

One comment on the thread adds `--full-refresh` to the CI selector. dbt's CI guide does not. A full refresh belongs in an explicit backfill, not in the pull-request job. Slim CI on a pull request still needs a warehouse credential. That credential is a deployment identity aimed at the CI schema, not a copy of the production password in the workflow file.
