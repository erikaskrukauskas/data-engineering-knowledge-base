# Partition column functions

A BigQuery partition filter compares the partition column itself.

## Look for

- `DATE(partition_ts)` or another function wrapped around the partition column.
- `WHERE` that ORs the partition column with a non-partition column.
- A filter the author reordered, believing the position of the predicate changed pruning. Conjunction order does not.

## Fix shape

- Filter the partition column directly.
- Keep the partition predicate in an AND.

## Field reports

No thread in the catalog communities stated this failure with a stack and a fix that matches the docs. Search [r/dataengineering](https://www.reddit.com/r/dataengineering/) and the tool tags in `sources/catalog.yaml` before treating a comment as a rule.

## Canonical check

- [Querying partitioned tables](https://docs.cloud.google.com/bigquery/docs/querying-partitioned-tables) — An AND of a partition predicate qualifies in any order. An OR with a non-partition column does not. Wrapping the partition column in a function blocks pruning.

A comment on a field report says predicate order matters. The official page says order does not for AND. The page wins.
