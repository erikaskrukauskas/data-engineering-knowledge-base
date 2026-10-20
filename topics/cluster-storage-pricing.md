# Clustering and long-term storage

An unpartitioned table needs 90 days without edits to qualify for long-term storage pricing. A partitioned table qualifies partition by partition.

## Look for

- A daily `MERGE` into an unpartitioned clustered table, with long-term storage assumed.
- A partition strategy avoided only to keep clustering.
- A table altered every day by a job that touches one recent day and rewrites the storage clock for the whole table.

## Fix shape

- Partition when new data arrives continually and old partitions should age into long-term pricing.
- Keep clustering inside each partition.

## Field reports

No thread in the catalog communities stated this failure with a stack and a fix that matches the docs. Search [r/dataengineering](https://www.reddit.com/r/dataengineering/) and the tool tags in `sources/catalog.yaml` before treating a comment as a rule.

## Canonical check

- [Clustered tables](https://cloud.google.com/bigquery/docs/clustered-tables) — Without partitioning, the entire table must be unedited for 90 consecutive days. Partitioning lets each partition qualify on its own.

Automatic reclustering is a free operation in the pricing sense the page describes, and it is still subject to quotas. It does not by itself partition the table.
