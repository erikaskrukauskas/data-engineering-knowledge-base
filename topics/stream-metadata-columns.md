# Stream metadata columns

A stream query returns the source columns plus change metadata. The load uses `METADATA$ACTION` and `METADATA$ISUPDATE` to tell an insert from an update.

## Look for

- `SELECT *` from a stream loaded into a table that has no metadata columns, so the insert fails or drops the action.
- An updated row treated as two business events because the before and after images were both inserted.
- `METADATA$ISUPDATE` ignored, so an insert-plus-update in one offset is stored as something other than the net new row.

## Fix shape

- Project the metadata columns into the consume statement.
- Treat the standard-stream delta as the net row. The page notes that an add followed by an update in the current offset is a new row with `METADATA$ISUPDATE` false.

## Field reports

No thread in the catalog communities stated this failure with a stack and a fix that matches the docs. Search [r/dataengineering](https://www.reddit.com/r/dataengineering/) and the tool tags in `sources/catalog.yaml` before treating a comment as a rule.

## Canonical check

- [Streams](https://docs.snowflake.com/en/user-guide/streams-intro) — A stream returns the source shape plus metadata columns, including `METADATA$ACTION`. An add followed by an update in the same offset is recorded as a new row.

Append-only streams do not produce the delete and update pairs a standard stream does. Do not look for those actions on an append-only stream.
