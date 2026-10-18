# Partition filters on views

A view over a table with `require_partition_filter` still requires the filter at query time.

## Look for

- A view created so analysts can query without a date predicate.
- `LIMIT` added in the view as a cost control.
- A scheduled query of the view with no partition predicate.

## Fix shape

- Keep the predicate in every query of the view, or materialize a bounded aggregate that is the thing analysts query.
- Do not use LIMIT as the cost control.

## Field reports

No thread in the catalog communities stated this failure with a stack and a fix that matches the docs. Search [r/dataengineering](https://www.reddit.com/r/dataengineering/) and the tool tags in `sources/catalog.yaml` before treating a comment as a rule.

## Canonical check

- [Querying partitioned tables](https://docs.cloud.google.com/bigquery/docs/querying-partitioned-tables) — Views inherit `require_partition_filter`. LIMIT does not reduce the bytes scanned.

The partition-filters note covers the base table. This note is the view on top of it.
