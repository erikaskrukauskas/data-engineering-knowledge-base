# Audit hook rollback

On Postgres and Redshift, a pre-hook insert rolls back with the model unless the hook is outside the transaction.

## Look for

- An audit table insert in `pre_hook` with no `before_begin`.
- A row expected in the audit table after a failed `dbt run`, and the table empty.
- The same pattern copied onto Snowflake, where the transaction flag must not be used.

## Fix shape

- On Postgres or Redshift, use `before_begin` for the audit insert.
- On Snowflake, BigQuery, and Spark, keep the hook as SQL without `transaction: false`.

## Field reports

No thread in the catalog communities stated this failure with a stack and a fix that matches the docs. Search [r/dataengineering](https://www.reddit.com/r/dataengineering/) and the tool tags in `sources/catalog.yaml` before treating a comment as a rule.

## Canonical check

- [Hooks](https://docs.getdbt.com/reference/resource-configs/pre-hook-post-hook) — The audit example is the reason to leave the transaction: the insert should remain if model creation fails. The transaction opt-out is for adapters that use transactions.

This is the same transaction mechanism as a VACUUM hook. The failure here is a missing audit row, not a VACUUM error.
