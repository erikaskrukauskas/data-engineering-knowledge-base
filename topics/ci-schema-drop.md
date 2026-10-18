# CI schema lifetime

Each pull request builds in its own schema. The schema is dropped when the pull request is merged or closed.

## Look for

- CI schemas named `ci` with no pull-request id, shared by every branch.
- Old `dbt_cloud_pr_*` or `pr_*` schemas left behind after merge.
- A CI job that writes the production custom schema because `generate_schema_name` ignores the target.

## Fix shape

- Use the environment-aware schema macro so non-prod runs stay in the target schema.
- Drop the pull-request schema at the end of the job. On the dbt platform this drop happens when the pull request is closed.

## Field reports

No thread in the catalog communities stated this failure with a stack and a fix that matches the docs. Search [r/dataengineering](https://www.reddit.com/r/dataengineering/) and the tool tags in `sources/catalog.yaml` before treating a comment as a rule.

## Canonical check

- [dbt CI](https://docs.getdbt.com/guides/set-up-ci) — Each pull request gets its own schema. dbt drops it after the pull request merges. CI does not block a production run.
- [Custom schemas](https://docs.getdbt.com/docs/build/custom-schemas) — In dev and CI, `generate_schema_name_for_env` keeps the target schema and does not use the production custom schema name.

Closing the pull request is what drops the platform schema. A job you cancel by hand may leave the schema. Drop it explicitly in a GitHub Action.
