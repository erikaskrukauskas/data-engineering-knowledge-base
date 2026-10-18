# Spark and table layout

Partition by a column the queries filter on, with low cardinality and enough data in each partition. Compact small files. The table format's transaction log owns the file list.

## Look for

- A partition column such as `user_id` with hundreds of thousands of values.
- Many small files from frequent small batches, and no compaction job.
- Spark code that reads a partition directory directly, or that adds and drops partitions by hand, on a Delta or Iceberg table.
- Jobs that cache a Delta table and then miss later commits.
- Writes that delete or replace Parquet files under the table path outside `MERGE`, `OPTIMIZE`, or the Iceberg API.
- A job that deletes the table directory and creates a new table at the same path. That delete is not atomic, and a reader can see a partial table.
- `dataChange` set to false on a write that changes row values. Delta documents that combination as a way to corrupt the table.

## Fix shape

- Partition by a date or another low-cardinality filter column when each partition holds on the order of 1 GB. Otherwise leave the table unpartitioned and use clustering when the platform provides it.
- Schedule compaction. On Delta that is `OPTIMIZE`, with `VACUUM` only under the retention the platform documents.
- Query the table with a `WHERE` clause so the engine skips files. Do not point `spark.read` at a single partition directory of a managed table.
- Replace a Delta table with `CREATE OR REPLACE` or an overwrite that sets `overwriteSchema` when the schema must change. Use `dataChange=false` only for compaction that does not change rows.
- Rebuild a late-arriving business date by replacing that partition, not by appending a second copy.

## Field reports

No single thread is the source of this rule. Search these when a Spark or lakehouse project fails on file counts, skew, or partition choice:

- [r/apachespark](https://www.reddit.com/r/apachespark/)
- [r/databricks](https://www.reddit.com/r/databricks/)
- [`pyspark` sorted by votes](https://stackoverflow.com/questions/tagged/pyspark?tab=Votes)
- [`apache-spark` sorted by votes](https://stackoverflow.com/questions/tagged/apache-spark?tab=Votes)
- [`delta-lake` sorted by votes](https://stackoverflow.com/questions/tagged/delta-lake?tab=Votes)

Keep a question when it includes the runtime version, the partition column, and the file counts or the error.

## Canonical check

- [Delta Lake best practices](https://docs.delta.io/best-practices/) — avoid high-cardinality partition columns, and compact small files
- [Databricks Delta Lake best practices](https://docs.databricks.com/aws/en/delta/best-practices) — do not edit files directly, and do not rely on Spark cache for Delta tables
- [Apache Iceberg with Spark](https://iceberg.apache.org/docs/latest/spark-getting-started/) — write with `writeTo` or `MERGE INTO`. The older DataFrame `write` API still works and is not the recommended path.

Apply the page that matches the table format in the repository. Delta advice is not automatically Iceberg advice.
