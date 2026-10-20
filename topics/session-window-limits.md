# Session window limits

A streaming session window uses an output mode other than update, and the group key includes at least one column besides the session window.

## Look for

- `session_window` with `outputMode("update")`.
- A group by of only the session window.
- Partial aggregation expected inside each partition. It is off unless `spark.sql.streaming.sessionWindow.merge.sessions.in.local.partition` is set.

## Fix shape

- Use append, with a watermark, for the session aggregation.
- Add the entity column to the group key.
- Enable the local merge flag only when many rows share a key inside a partition and the extra sort is worth it.

## Field reports

No thread in the catalog communities stated this failure with a stack and a fix that matches the docs. Search [r/dataengineering](https://www.reddit.com/r/dataengineering/) and the tool tags in `sources/catalog.yaml` before treating a comment as a rule.

## Canonical check

- [Structured Streaming](https://spark.apache.org/docs/latest/streaming/apis-on-dataframes-and-datasets.html) — Update mode is not supported for session windows. The grouping key needs a column besides `session_window`. Partial aggregation is off by default.

A batch query may group by the session window alone. The extra column rule is for the streaming query.
