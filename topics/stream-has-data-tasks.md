# Stream has-data checks

When `SYSTEM$STREAM_HAS_DATA` is true, consume the stream. A false positive otherwise keeps tasks from skipping and keeps the warehouse running.

## Look for

- A task `WHEN SYSTEM$STREAM_HAS_DATA` that never reaches a DML consume.
- `SELECT COUNT(*)` from the stream returning zero while the function still returns true.
- No follow-up consume, so the next task run is charged again.

## Fix shape

- On a false positive, consume with `CREATE TEMPORARY TABLE ... AS SELECT * FROM stream WHERE 1=0`.
- That CTAS is DML, advances the offset, and writes nothing.

## Field reports

No thread in the catalog communities stated this failure with a stack and a fix that matches the docs. Search [r/dataengineering](https://www.reddit.com/r/dataengineering/) and the tool tags in `sources/catalog.yaml` before treating a comment as a rule.

## Canonical check

- [Manage streams](https://docs.snowflake.com/en/user-guide/streams-manage) — A true result should be consumed even if it is a false positive. Otherwise tasks with that function in `WHEN` do not skip. A CTAS with `WHERE 1=0` advances the offset without processing rows.

Calling the function on an empty stream that returns false also prevents staleness. A true result does not. It has to be consumed.
