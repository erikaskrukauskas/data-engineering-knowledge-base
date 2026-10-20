# Standard stream net changes

A standard stream returns the net change. An insert followed by a delete in the same offset disappears.

## Look for

- An audit built on a standard stream that must see every delete.
- A row inserted and deleted between task runs, missing from the stream, and treated as a bug.
- Geospatial columns read through a standard stream.

## Fix shape

- Use a standard stream when the pipeline wants the net row.
- Use an append-only stream when every insert must be kept, including rows deleted before the offset advances.
- Use append-only for geospatial data. Standard streams cannot retrieve change data for it.

## Field reports

No thread in the catalog communities stated this failure with a stack and a fix that matches the docs. Search [r/dataengineering](https://www.reddit.com/r/dataengineering/) and the tool tags in `sources/catalog.yaml` before treating a comment as a rule.

## Canonical check

- [Streams](https://docs.snowflake.com/en/user-guide/streams-intro) — A standard stream joins inserts and deletes and omits a row inserted and deleted in the same interval. Append-only records inserts only. Geospatial data needs an append-only stream.

Append-only ignores updates, deletes, and truncates. After you consume it, the source table can be truncated without those deletes showing up in the next read.
