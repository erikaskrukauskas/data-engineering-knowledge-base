# Stream staleness

Consume a Snowflake stream with DML before `STALE_AFTER`. A stale stream cannot see the missed changes and must be recreated.

## Look for

- A stream with `STALE` true or `STALE_AFTER` in the past.
- A source table recreated with `CREATE OR REPLACE`, which drops history and stales every stream on it.
- A view stream whose underlying table was replaced.
- A cloned database whose stream clone cannot read the unconsumed records.

## Fix shape

- Consume the stream on a schedule inside the retention window.
- Check `STALE_AFTER` with `SHOW STREAMS` or `DESCRIBE STREAM`.
- Recreate the stream after a stale state or after `CREATE OR REPLACE` on the source.

## Field reports

No thread in the catalog communities stated this failure with a stack and a fix that matches the docs. Search [r/dataengineering](https://www.reddit.com/r/dataengineering/) and the tool tags in `sources/catalog.yaml` before treating a comment as a rule.

## Canonical check

- [Streams](https://docs.snowflake.com/en/user-guide/streams-intro) — The stream is stale when its offset falls outside retention. Unconsumed changes are then gone. `CREATE OR REPLACE` on the source drops history and stales the stream. Rename does not.

Snowflake can extend retention up to `MAX_DATA_EXTENSION_TIME_IN_DAYS`, 14 by default, while a stream is unconsumed. That extension is not a promise past `STALE_AFTER`. Shared-table streams do not extend the source retention.
