# AQE parallelism first

`spark.sql.adaptive.coalescePartitions.parallelismFirst` is turned off when the cluster is busy and small tasks are the problem.

## Look for

- The default true on a shared cluster where every query launches a large number of tiny tasks.
- The flag set false on a small cluster where you needed the extra parallelism and queries slowed down.
- Advisory size edited while `parallelismFirst` is still true, so the advisory size is ignored.

## Fix shape

- Set `parallelismFirst` to false on a busy cluster so coalescing honors the 64 MB advisory size.
- Leave it true when maximizing task count matters more than task size.

## Field reports

No thread in the catalog communities stated this failure with a stack and a fix that matches the docs. Search [r/dataengineering](https://www.reddit.com/r/dataengineering/) and the tool tags in `sources/catalog.yaml` before treating a comment as a rule.

## Canonical check

- [Spark SQL performance tuning](https://spark.apache.org/docs/latest/sql-performance-tuning) — The default is true, which ignores the advisory size and uses the 1 MB minimum. The page recommends false on a busy cluster.

This flag only matters when AQE and coalescing are both enabled.
