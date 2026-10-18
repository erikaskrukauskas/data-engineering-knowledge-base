# Grant clobber

A more specific `grants` block replaces the project list unless the privilege name starts with `+`.

## Look for

- `grants={'select': ['one_user']}` on a model when `dbt_project.yml` already grants select to the reporting roles.
- An empty `select: []` checked in by mistake, which revokes everyone.
- The `+grants` key removed to stop a bad grant. Removing the key leaves the database grants as they are.

## Fix shape

- Use `+select` when the model should add a grantee.
- Use `select: []` only when the intent is to revoke.
- Read the grant and revoke statements in the debug log after the build.

## Field reports

No thread in the catalog communities stated this failure with a stack and a fix that matches the docs. Search [r/dataengineering](https://www.reddit.com/r/dataengineering/) and the tool tags in `sources/catalog.yaml` before treating a comment as a rule.

## Canonical check

- [dbt grants](https://docs.getdbt.com/reference/resource-configs/grants) — The more specific config replaces grantees for that privilege. Prefix the privilege with `+` to append. Deleting the config stops dbt from managing grants. An empty list revokes them.

BigQuery privileges are role names such as `roles/bigquery.dataViewer`, and grantees need a `user:` or `group:` prefix. `select` is the wrong key on BigQuery.
