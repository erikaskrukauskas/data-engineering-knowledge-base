# Insert-only external streams

An insert-only stream on an external table returns rows from files added in the interval. A file removed from storage does not become a delete.

## Look for

- A stream on an external table used to propagate object deletes.
- A removed file expected to produce change rows.
- An append-only stream created on an external table. External tables use insert-only streams.

## Fix shape

- Track storage deletes with the external table refresh and a separate compare.
- Use the insert-only stream for new files only.

## Field reports

No thread in the catalog communities stated this failure with a stack and a fix that matches the docs. Search [r/dataengineering](https://www.reddit.com/r/dataengineering/) and the tool tags in `sources/catalog.yaml` before treating a comment as a rule.

## Canonical check

- [Streams](https://docs.snowflake.com/en/user-guide/streams-intro) — If one file is removed and another is added, an insert-only stream returns the new file's rows only. Historical files in cloud storage are not governed by Snowflake retention.

Standard and append-only streams are for Snowflake-managed tables, dynamic tables, views, and Snowflake-managed Iceberg, not for this external-file case.
