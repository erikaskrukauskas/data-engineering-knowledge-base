# Skew threshold

Spark treats a sort-merge partition as skewed when it is over 256 MB and more than five times the median.

## Look for

- A key called skewed because one task is slower, with the partition still under 256 MB.
- `skewedPartitionFactor` set to 1 so every partition above the median is split.
- Skew handling disabled and a null join key that collects into one task.

## Fix shape

- Keep the defaults unless a measured straggler is above both thresholds and still not split.
- Filter or bucket a null-heavy key before lowering the threshold for the whole session.

## Field reports

No thread in the catalog communities stated this failure with a stack and a fix that matches the docs. Search [r/dataengineering](https://www.reddit.com/r/dataengineering/) and the tool tags in `sources/catalog.yaml` before treating a comment as a rule.

## Canonical check

- [Spark SQL performance tuning](https://spark.apache.org/docs/latest/sql-performance-tuning) — The factor default is 5 and the size default is 256 MB. Both have to be true. Skew join handling defaults to on when AQE is on.

`forceOptimizeSkewedJoin` defaults to false. Turning it on adds a shuffle. Use it for a job, not as a cluster default.
