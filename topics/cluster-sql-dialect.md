# Clustering SQL dialect

Querying a clustered table and writing query results into one are GoogleSQL operations.

## Look for

- A legacy SQL job pointed at a clustered table.
- A load path that creates the clustered table from a non-GoogleSQL query result.
- A client still sending the older SQL dialect for the clustered mart.

## Fix shape

- Run the readers and the writers that produce clustered results in GoogleSQL.

## Field reports

No thread in the catalog communities stated this failure with a stack and a fix that matches the docs. Search [r/dataengineering](https://www.reddit.com/r/dataengineering/) and the tool tags in `sources/catalog.yaml` before treating a comment as a rule.

## Canonical check

- [Clustered tables](https://cloud.google.com/bigquery/docs/clustered-tables) — Only GoogleSQL is supported for querying clustered tables and for writing query results to them.

Batch load, table copy, automatic reclustering, and export are listed as free operations that still count against quotas. They are not a second SQL dialect.
