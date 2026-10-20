# Stream stale-after visibility

`STALE_AFTER` can be calculated when the role can read the schema or database retention that applies to the source.

## Look for

- `SHOW STREAMS` with `STALE_AFTER` null for a role that does not own the source schema.
- A monitor that alerts only on the `STALE` column and ignores a past `STALE_AFTER`.
- A retention set on the schema, and a task role that cannot see it.

## Fix shape

- Grant the monitoring role access to the schema or database that holds the retention.
- Treat a past `STALE_AFTER` as the time to consume or recreate, even if `STALE` is not true yet.

## Field reports

No thread in the catalog communities stated this failure with a stack and a fix that matches the docs. Search [r/dataengineering](https://www.reddit.com/r/dataengineering/) and the tool tags in `sources/catalog.yaml` before treating a comment as a rule.

## Canonical check

- [Streams](https://docs.snowflake.com/en/user-guide/streams-intro) — If retention is set on the schema or database, the current role needs access there to calculate `STALE_AFTER`. After that timestamp the stream can go stale at any moment. The `STALE` flag can lag.

Reading the stream can still succeed for a while after `STALE_AFTER`. The page says not to rely on that. `SYSTEM$STREAM_HAS_DATA` can return unexpected results then.
