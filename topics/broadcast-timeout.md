# Broadcast timeout

A broadcast that cannot finish inside `spark.sql.broadcastTimeout` is the wrong strategy for that side.

## Look for

- A `BROADCAST` hint on a side that is over 10 MB and then a timeout at 300 seconds.
- The timeout raised to an hour so the hint can force a bad broadcast.
- The threshold set to -1 and a hint still applied, which does not make the side small.

## Fix shape

- Remove the hint or the oversized side.
- Raise the timeout only when the side is known to be small and the cluster is slow to transfer it.

## Field reports

No thread in the catalog communities stated this failure with a stack and a fix that matches the docs. Search [r/dataengineering](https://www.reddit.com/r/dataengineering/) and the tool tags in `sources/catalog.yaml` before treating a comment as a rule.

## Canonical check

- [Spark SQL performance tuning](https://spark.apache.org/docs/latest/sql-performance-tuning) — The broadcast timeout defaults to 300 seconds. A hint prioritizes broadcast even when statistics say the side is above the 10 MB threshold.

AQE can still convert a sort-merge join to a broadcast later, using the adaptive threshold. A hint is the path that forces it up front.
