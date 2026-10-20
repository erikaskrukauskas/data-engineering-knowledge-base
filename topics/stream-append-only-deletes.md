# Append-only stream deletes

An append-only stream returns the inserted rows even when some of those rows are deleted before the offset advances.

## Look for

- An append-only stream used as a net-change feed.
- A delete expected to cancel an insert in the same stream read.
- A truncate of the source expected to appear as delete records. Append-only does not capture truncates.

## Fix shape

- Use a standard stream when deletes must cancel inserts.
- Use append-only when the load is insert-only and a later truncate of the source should not be replayed as deletes.

## Field reports

No thread in the catalog communities stated this failure with a stack and a fix that matches the docs. Search [r/dataengineering](https://www.reddit.com/r/dataengineering/) and the tool tags in `sources/catalog.yaml` before treating a comment as a rule.

## Canonical check

- [Streams](https://docs.snowflake.com/en/user-guide/streams-intro) — If ten rows are inserted and five are deleted before the append-only offset advances, the stream still returns the ten inserts. Updates, deletes, and truncates are not captured.

Insert-only streams, used on external tables and some Iceberg and Delta Direct tables, are a third type. They also skip deletes, and cloud-storage history is not guaranteed.
