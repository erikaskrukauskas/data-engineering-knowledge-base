# Clustering column order

BigQuery clustering column order is the filter order. A query that skips the first clustered column is not the optimized case.

## Look for

- `CLUSTER BY country, order_date` while almost every query filters on `order_date` alone.
- More than four clustering columns.
- A STRING clustering column longer than 1024 characters, with the sort expected to use the whole value.

## Fix shape

- Put the most common filter column first.
- Stop at four clustering columns. Partition if you need another cut.
- Expect only the first 1024 characters of a STRING to participate.

## Field reports

No thread in the catalog communities stated this failure with a stack and a fix that matches the docs. Search [r/dataengineering](https://www.reddit.com/r/dataengineering/) and the tool tags in `sources/catalog.yaml` before treating a comment as a rule.

## Canonical check

- [Clustered tables](https://cloud.google.com/bigquery/docs/clustered-tables) — The first clustering column takes precedence. A filter that uses only later columns is not the optimized path. At most four clustering columns are allowed. STRING clustering uses the first 1024 characters.

Clustering sorts storage blocks, not individual rows. Equality on a later column can still prune some blocks. It is not the order the page recommends.
