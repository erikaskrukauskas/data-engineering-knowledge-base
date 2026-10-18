# AQE partition coalesce

Coalescing stays enabled so a large starting partition count does not become a large number of tiny tasks.

## Look for

- `spark.sql.adaptive.enabled` or `spark.sql.adaptive.coalescePartitions.enabled` set to false on Spark 3.2 or later.
- A hand-tuned partition count that is now too low because coalescing was the thing that made 200 safe.
- Advisory size raised far above 64 MB so coalescing produces a few huge tasks.

## Fix shape

- Keep both flags at their default true.
- Change `spark.sql.adaptive.advisoryPartitionSizeInBytes` from 64 MB only for a job whose partitions are the wrong size after a measurement.

## Field reports

No thread in the catalog communities stated this failure with a stack and a fix that matches the docs. Search [r/dataengineering](https://www.reddit.com/r/dataengineering/) and the tool tags in `sources/catalog.yaml` before treating a comment as a rule.

## Canonical check

- [Spark SQL performance tuning](https://spark.apache.org/docs/latest/sql-performance-tuning) — AQE has been on by default since Spark 3.2. Coalescing is on by default and targets the advisory partition size, 64 MB.

`parallelismFirst` defaults to true and ignores that 64 MB target in favor of a 1 MB minimum, to keep more tasks. On a busy cluster the docs recommend turning `parallelismFirst` off. That is the next note.
