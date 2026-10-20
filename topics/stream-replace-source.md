# Replacing a streamed table

Replacing a source table or an underlying table of a streamed view stales the stream. The stream is recreated after the replace.

## Look for

- `CREATE OR REPLACE TABLE` in a dbt full refresh of a table that has a stream.
- A view stream that goes stale after one underlying table is rebuilt.
- A pipeline that renames and swaps, which is safe, confused with replace, which is not.

## Fix shape

- Consume and then recreate the stream after a replace.
- Prefer a rename when the history must stay attached. Rename does not stale the stream.

## Field reports

No thread in the catalog communities stated this failure with a stack and a fix that matches the docs. Search [r/dataengineering](https://www.reddit.com/r/dataengineering/) and the tool tags in `sources/catalog.yaml` before treating a comment as a rule.

## Canonical check

- [Streams](https://docs.snowflake.com/en/user-guide/streams-intro) — `CREATE OR REPLACE` drops history and stales streams on the table or view. Renaming the source does not. A dropped object recreated under the same name is not the object the stream tracks.

A clone of the database does not carry readable unconsumed stream records. The clone's history starts at the clone.
