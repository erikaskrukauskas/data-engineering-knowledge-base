# Clustering existing data

`ALTER TABLE` clustering applies to new data. Existing bytes stay unclustered until something rewrites them.

## Look for

- A table altered to `CLUSTER BY` and queried as if yesterday's blocks were sorted.
- No rewrite after the alter.
- A dry run used as proof the clustering worked. Clustered queries do not have an accurate cost estimate before execution.

## Fix shape

- Rewrite existing data if the old blocks must be clustered. The clustering page points at an UPDATE for that reclustering.
- Judge the change from bytes processed after execution, not from the estimate.

## Field reports

No thread in the catalog communities stated this failure with a stack and a fix that matches the docs. Search [r/dataengineering](https://www.reddit.com/r/dataengineering/) and the tool tags in `sources/catalog.yaml` before treating a comment as a rule.

## Canonical check

- [Clustered tables](https://cloud.google.com/bigquery/docs/clustered-tables) — Existing data is not automatically clustered when you alter a non-clustered table. Only new data is. Automatic reclustering maintains data written under the clustered spec. Cost estimates for clustered queries are not accurate before the query runs.

Automatic reclustering does not change query slot capacity. Clustering does not guarantee fewer slots.
