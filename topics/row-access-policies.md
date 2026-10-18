# Row access policies

A row filter is a platform policy or a Unity Catalog filter. It does not add a second full scan of another table unless the policy says it does.

## Look for

- A mart that copies `WHERE region = ...` into every model instead of a row access policy.
- A BigQuery row access policy whose predicate subqueries another large table.
- A Unity Catalog row filter maintained as a different function on every table when the rule is the same.

## Fix shape

- On BigQuery, attach a row access policy to the table. Keep the predicate on columns of that table.
- On Databricks, use a table row filter for one table and an ABAC policy when the same rule applies to many tagged tables.

## Field reports

- [Row level security cost](https://www.reddit.com/r/bigquery/comments/1jjfcfb/row_level_security/) — r/bigquery. The failure people were about to build was a hand-rolled filter. The reply that matches the platform is the built-in row access policy, with the cost caveat when the policy subquery reads another table.

## Canonical check

- [Databricks row filters and column masks](https://docs.databricks.com/aws/en/data-governance/unity-catalog/filters-and-masks) — A row filter is a SQL function evaluated at query time. ABAC policies attach at catalog or schema scope and apply to tagged tables.

BigQuery documents that a row-level policy is not an extra charge by itself, and that a policy subquery against another table can change the bytes scanned. Confirm that page for the project's region before promising the cost.
