# BigQuery grant roles

On BigQuery, `grants` names an IAM role and a prefixed principal.

## Look for

- `select: ['analyst']` on a BigQuery model.
- A principal with no `user:`, `group:`, or `serviceAccount:` prefix.
- `grant_access_to` used when the intent was a grant, or the reverse. Authorized views are the other config.

## Fix shape

- Grant `roles/bigquery.dataViewer` to `group:` or `serviceAccount:` principals.
- Use `grant_access_to` only for an authorized view.

## Field reports

No thread in the catalog communities stated this failure with a stack and a fix that matches the docs. Search [r/dataengineering](https://www.reddit.com/r/dataengineering/) and the tool tags in `sources/catalog.yaml` before treating a comment as a rule.

## Canonical check

- [dbt grants](https://docs.getdbt.com/reference/resource-configs/grants) — BigQuery privileges are IAM roles. Grantees need a principal prefix. `grant_access_to` is the authorized-view config, separate from `grants`.

A grant config does nothing on a relation the adapter cannot grant on. Spark without table ACL is that case.
