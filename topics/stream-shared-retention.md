# Streams on shared tables

Streams on shared tables or views do not lengthen the source retention. Consume them inside the retention the provider already set.

## Look for

- A stream on a shared table left unconsumed for the 14-day extension that applies to local tables.
- A consumer account assuming `MAX_DATA_EXTENSION_TIME_IN_DAYS` protects the share.
- A stale shared stream recreated without telling the downstream job the missed changes are gone.

## Fix shape

- Consume shared streams inside the provider retention.
- Recreate a stale stream and accept that the gap is not recoverable from the stream.

## Field reports

No thread in the catalog communities stated this failure with a stack and a fix that matches the docs. Search [r/dataengineering](https://www.reddit.com/r/dataengineering/) and the tool tags in `sources/catalog.yaml` before treating a comment as a rule.

## Canonical check

- [Streams](https://docs.snowflake.com/en/user-guide/streams-intro) — Streams on shared tables or views do not extend data retention for the table or the view's underlying tables.

The local-table extension, up to 14 days by default, is a different rule. It does not transfer to a share.
