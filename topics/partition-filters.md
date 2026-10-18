# Partition filters

A query against a large partitioned table names the partition column in a predicate that can drop partitions. `LIMIT` does not do that.

## Look for

- A partitioned table created without `require_partition_filter` (BigQuery) or an equivalent guard, and models that `select` from it with no `WHERE` on the partition column.
- A predicate that wraps the partition column in a function, or an `OR` between the partition column and another column. BigQuery will not prune on that predicate.
- `LIMIT` used as the cost control on a warehouse that bills bytes scanned.
- A dbt model of a partitioned table whose incremental filter is on an extraction timestamp while the table is partitioned on a business date, so the prune misses the partition that needs to change.

## Fix shape

- Partition the large table on the column queries actually filter, often a date.
- Require a partition filter on the table. In the model, filter that column with a predicate BigQuery can use for elimination: the column compared directly, combined with other predicates by `AND`.
- For ingestion-time partitions, filter `_PARTITIONTIME` or `_PARTITIONDATE`.
- Estimate bytes with a dry run before running an ad hoc query. Dry runs are not billed as query slots.

## Field reports

- [Why LIMIT and DISTINCT do not cut BigQuery cost](https://www.reddit.com/r/dataengineering/comments/1gnyqhx/why_is_neither_distinct_nor_limit_encouraged_in/) — r/dataengineering. `LIMIT` is applied after the scan, so the job is billed for the bytes read. The docs' fix is a predicate on the partition column, not a row cap.

## Canonical check

- [Query partitioned tables](https://docs.cloud.google.com/bigquery/docs/querying-partitioned-tables) — with `require_partition_filter`, a query that has no eliminating predicate fails. A predicate that `OR`s the partition column with another column does not qualify. Views over the table inherit the requirement.
- [Optimize query computation](https://docs.cloud.google.com/bigquery/docs/best-practices-performance-compute) — filter `_PARTITIONTIME` or the partitioning column so only those partitions are scanned.

A comment on r/bigquery says the partition predicate must be written first or BigQuery will not prune. The partition-query page says an `AND` of a qualifying partition predicate and another predicate qualifies regardless of order. Apply the page, and still avoid wrapping the partition column in a function.
