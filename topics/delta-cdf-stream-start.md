# Change feed stream start

A streaming read with `readChangeFeed` and no `startingVersion` or `startingTimestamp` returns the latest snapshot as inserts, then later changes.

## Look for

- `readStream` with `readChangeFeed` true and no starting version, expected to emit nothing until the next write.
- A starting version past the last commit without `spark.databricks.delta.changeDataFeed.timestampOutOfRange.enabled`.
- Rate limits assumed to split one commit. They do not, except for the starting snapshot.

## Fix shape

- Set `startingVersion` or `startingTimestamp` to the first change you want.
- Turn on the out-of-range option only when an empty read is preferable to `timestampGreaterThanLatestCommit`.

## Field reports

No thread in the catalog communities stated this failure with a stack and a fix that matches the docs. Search [r/dataengineering](https://www.reddit.com/r/dataengineering/) and the tool tags in `sources/catalog.yaml` before treating a comment as a rule.

## Canonical check

- [Change data feed](https://docs.delta.io/delta-change-data-feed/) — Omitting the start returns the latest snapshot as inserts. A start beyond the last commit throws unless the out-of-range option is true, in which case the read is empty. Rate limits apply to a whole commit after the starting snapshot.

Batch `table_changes` is inclusive of the start and end versions. A one-version window still returns that version.
