# Spark skew joins

Adaptive query execution splits skewed sort-merge partitions when skew handling is on.

## Look for

- `spark.sql.adaptive.enabled` or `spark.sql.adaptive.skewJoin.enabled` set to false.
- A join key such as a null or a single customer that dominates the rows, with no skew handling and no salted key.
- `spark.sql.adaptive.forceOptimizeSkewedJoin` turned on for every query, including ones that should not pay an extra shuffle.

## Fix shape

- Leave AQE and skew-join splitting at their defaults unless a measured job needs a different `skewedPartitionFactor` or threshold.
- The defaults treat a partition as skewed when it is both over 256 MB and more than 5 times the median.

## Field reports

No thread in the catalog communities stated this failure with a stack and a fix that matches the docs. Search [r/dataengineering](https://www.reddit.com/r/dataengineering/) and the tool tags in `sources/catalog.yaml` before treating a comment as a rule.

## Canonical check

- [Spark SQL performance tuning](https://spark.apache.org/docs/latest/sql-performance-tuning) — Skew handling runs when both `spark.sql.adaptive.enabled` and `spark.sql.adaptive.skewJoin.enabled` are true. It splits skewed sort-merge tasks. Both default to on.

`forceOptimizeSkewedJoin` defaults to false because the extra shuffle is not free. Turn it on for a job that is stuck on a straggler, not as a cluster-wide default.
