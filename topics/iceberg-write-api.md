# Iceberg write API

New Iceberg writes use `writeTo` or `MERGE INTO`. The old DataFrame `write` path is not the one the getting-started guide recommends.

## Look for

- `df.write.format("iceberg").mode("append").save(...)` copied from a Hive job.
- A partition overwrite that uses the Hive-style writer.
- A schema change done by writing files instead of by evolution.

## Fix shape

- Use `writeTo` for append and replace.
- Use `MERGE INTO` when rows must be updated in place.

## Field reports

No thread in the catalog communities stated this failure with a stack and a fix that matches the docs. Search [r/dataengineering](https://www.reddit.com/r/dataengineering/) and the tool tags in `sources/catalog.yaml` before treating a comment as a rule.

## Canonical check

- [Iceberg Spark getting started](https://iceberg.apache.org/docs/latest/spark-getting-started/) — The recommended Spark writes are `writeTo` and `MERGE INTO`. The older DataFrame write API is not the recommended path.

The getting-started page is about Iceberg tables. A Delta table still uses the Delta write path. Do not point a Delta job at Iceberg options.
