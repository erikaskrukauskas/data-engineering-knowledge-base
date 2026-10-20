# Hooks inside a transaction

On Postgres and Redshift, hooks run in the model transaction unless you opt out. VACUUM and an audit insert that must survive a failed model do not belong inside it.

## Look for

- A Redshift `VACUUM` in `post_hook` with no `transaction: false`.
- An audit insert in `pre_hook` that disappears when the model rolls back.
- `before_begin` or `after_commit` missing on the statement that must outlive the model.

## Fix shape

- Use `before_begin` or `after_commit`, or a hook dict with `transaction: false`, on Postgres and Redshift.
- Keep ordinary hooks inside the transaction when they should roll back with the model.

## Field reports

No thread in the catalog communities stated this failure with a stack and a fix that matches the docs. Search [r/dataengineering](https://www.reddit.com/r/dataengineering/) and the tool tags in `sources/catalog.yaml` before treating a comment as a rule.

## Canonical check

- [Hooks](https://docs.getdbt.com/reference/resource-configs/pre-hook-post-hook) — Postgres and Redshift run hooks in the model transaction by default. VACUUM cannot run there. An audit row that must persist uses a hook outside the transaction.

Do not set `transaction: false` on Snowflake, BigQuery, Spark, or Databricks. The hook page says that syntax is for adapters where dbt uses transactions.
