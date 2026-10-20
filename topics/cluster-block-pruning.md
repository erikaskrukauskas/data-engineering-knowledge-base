# Clustering block pruning

BigQuery prunes clustered blocks when the filter is on the clustering columns. The bytes billed are the columns read in the blocks that remain.

## Look for

- A function wrapped around the clustering column.
- A query that references the clustered table several times, each with a different filter, and a cost model that counts the scan once.
- A clustering column of type ARRAY or STRUCT. Cluster columns have to be top-level and non-repeated, of a supported scalar type.

## Fix shape

- Filter the clustering column directly.
- Expect each reference in the query to scan its own blocks.
- Cluster only on a supported top-level type: numeric, bool, date, datetime, geography, string, timestamp, range, or bignumeric.

## Field reports

No thread in the catalog communities stated this failure with a stack and a fix that matches the docs. Search [r/dataengineering](https://www.reddit.com/r/dataengineering/) and the tool tags in `sources/catalog.yaml` before treating a comment as a rule.

## Canonical check

- [Clustered tables](https://cloud.google.com/bigquery/docs/clustered-tables) — Filters on clustered columns prune blocks. Bytes processed are the referenced columns in the scanned blocks. Repeated references are charged per filter. Cluster columns must be top-level non-repeated values of the listed types.

Pruning is block-level. A selective filter can still read a whole block whose min and max cover the predicate.
