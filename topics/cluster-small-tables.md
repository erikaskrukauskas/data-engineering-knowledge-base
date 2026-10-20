# Clustering small tables

Cluster an unpartitioned table over 64 MB, or a partition over 64 MB. Smaller objects can be clustered, and the gain is usually negligible.

## Look for

- `CLUSTER BY` on a dimension of a few megabytes.
- A partitioned table whose partitions average well under 10 GB, plus clustering, plus a complaint about metadata.
- Clustering used instead of a partition filter on a multi-terabyte table that also needs a cost estimate before the query runs.

## Fix shape

- Cluster the large table or the large partitions.
- Add partitioning when you need a cost estimate before execution, and keep the average partition at least 10 GB.

## Field reports

No thread in the catalog communities stated this failure with a stack and a fix that matches the docs. Search [r/dataengineering](https://www.reddit.com/r/dataengineering/) and the tool tags in `sources/catalog.yaml` before treating a comment as a rule.

## Canonical check

- [Clustered tables](https://cloud.google.com/bigquery/docs/clustered-tables) — Tables or partitions larger than 64 MB are the ones likely to benefit. Partitions should average at least 10 GB when you also partition. Many small partitions increase metadata.

Clustering can sit on an unpartitioned table. Partitioning is the extra step when you need the pre-run cost estimate or long-term storage per partition.
