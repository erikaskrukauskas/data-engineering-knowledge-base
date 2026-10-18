# Model grants

After a model builds, the only identities that can read it are the ones named in its grants. The identity that runs the job is the one that writes.

## Look for

- Marts with no `grants` config, in a warehouse where the creating role's default privileges let every user in the database read the new table.
- A personal developer user as the production job user.
- `grants` copied onto a model without `+select`, so the model's list replaces the project list instead of adding to it.
- A BigQuery grant of `select` rather than `roles/bigquery.dataViewer`, or a grantee with no `user:`, `group:`, or `serviceAccount:` prefix.
- Grants managed in dbt and also granted to future objects in a hook that the next build then revokes, because dbt makes the object match the config exactly.
- An empty `grants` key that was deleted to "stop managing access." Deleting the key leaves existing grants in place. An empty list revokes them.

## Fix shape

- Set `grants` on marts in `dbt_project.yml`, and override a single model with `+select` when adding a grantee.
- Use a service account for the production job. Grant people `select` on the mart schema, not write.
- On BigQuery, grant `roles/bigquery.dataViewer` to a prefixed principal. Use `grant_access_to` when the object is an authorized view over another dataset. That config is not `grants`.
- For row filters, column masks, or grants on objects that are not tables or views, use a hook. dbt's `grants` config does not cover those.

## Field reports

- [Who can read the source tables developers build from?](https://www.reddit.com/r/dataengineering/comments/1d4fl1j/how_does_your_team_grant_access_to_source_tables/) — r/dataengineering. Developers had been granted select on landed tables. The pattern that matches the docs is a service account that writes production, and reader roles granted on the schemas people are supposed to query.

## Canonical check

- [dbt grants](https://docs.getdbt.com/reference/resource-configs/grants) — after the model builds, dbt grants and revokes until the object matches the config. A more specific `grants` block replaces the project list unless the privilege is prefixed with `+`. Removing the config stops dbt from changing grants. `select: []` revokes them.
- Spark and open-source Delta do not support this config. Databricks needs table access control enabled before grants apply. Redshift group and role grantees need a `group:` or `role:` prefix.

Do not put a production password in the grant list. The grantee is a role or group name. The secret that lets the job run is the secrets-and-CI topic.
