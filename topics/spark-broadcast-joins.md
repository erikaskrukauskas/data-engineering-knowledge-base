# Spark broadcast joins

Spark broadcasts a join side at or under 10 MB unless a hint or AQE says otherwise.

## Look for

- `.hint("broadcast")` or a `BROADCAST` hint on a fact table.
- `spark.sql.autoBroadcastJoinThreshold` set to -1 with no comment, so no side is broadcast.
- A broadcast that times out at the default 300 seconds because the side was larger than expected.

## Fix shape

- Hint broadcast only for the small side. Leave the threshold at 10 MB until statistics show a larger side is still safe.
- Let AQE convert a sort-merge join to a broadcast when the runtime size is under `spark.sql.adaptive.autoBroadcastJoinThreshold`.

## Field reports

No thread in the catalog communities stated this failure with a stack and a fix that matches the docs. Search [r/dataengineering](https://www.reddit.com/r/dataengineering/) and the tool tags in `sources/catalog.yaml` before treating a comment as a rule.

## Canonical check

- [Spark SQL performance tuning](https://spark.apache.org/docs/latest/sql-performance-tuning) — The default broadcast threshold is 10 MB. A hint forces that side even when statistics say it is larger. Setting the threshold to -1 disables broadcast.

A hint overrides statistics. A stale or missing statistic plus a hint is how a large table gets shipped to every executor.
