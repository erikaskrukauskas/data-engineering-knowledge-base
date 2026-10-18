# Shuffle partitions

`spark.sql.shuffle.partitions` stays at least the default 200 unless a measured job needs a different starting count.

## Look for

- The value set to 1 or to the size of the cluster on a shared Spark session.
- A job that disables AQE and then keeps 200 partitions of a few kilobytes.
- A join that broadcasts nothing and shuffles both sides into a handful of partitions.

## Fix shape

- Leave the default when AQE coalescing is on.
- Set a larger initial count and let coalescing reduce it, instead of guessing the final count.

## Field reports

No thread in the catalog communities stated this failure with a stack and a fix that matches the docs. Search [r/dataengineering](https://www.reddit.com/r/dataengineering/) and the tool tags in `sources/catalog.yaml` before treating a comment as a rule.

## Canonical check

- [Spark SQL performance tuning](https://spark.apache.org/docs/latest/sql-performance-tuning) — The default shuffle partition count is 200. With AQE coalescing, Spark picks a smaller count at runtime from that starting point.

`spark.sql.adaptive.coalescePartitions.initialPartitionNum` overrides the starting count when it is set. An unset value means the shuffle-partition setting is the start.
