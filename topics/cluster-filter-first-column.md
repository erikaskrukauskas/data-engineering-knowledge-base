# First cluster column filters

A multi-column cluster spec is chosen from the filters queries actually use. The leading column is the one those filters include.

## Look for

- `CLUSTER BY status, country` while the workload filters on `country` and never on `status`.
- A four-column cluster list that hides the selective column in position four.
- A partition column repeated as the only cluster column when the remaining filters are on a different high-cardinality column.

## Fix shape

- Order cluster columns so the first is the common filter.
- Cluster a partitioned table by the column used inside the partition, such as a customer id, when the partition is the date.

## Field reports

No thread in the catalog communities stated this failure with a stack and a fix that matches the docs. Search [r/dataengineering](https://www.reddit.com/r/dataengineering/) and the tool tags in `sources/catalog.yaml` before treating a comment as a rule.

## Canonical check

- [Clustered tables](https://cloud.google.com/bigquery/docs/clustered-tables) — In a cluster order of date, country, and status, a filter on date and country is the optimized case. A filter on only country and status is not. Partition plus cluster is how a date partition and a customer id cluster work together.

Block pruning can still skip some blocks on a later column. The page's rule is which filter is the optimized one.
