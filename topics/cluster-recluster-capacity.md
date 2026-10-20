# Reclustering and slots

Background reclustering keeps clustered blocks sorted. It does not increase the slot capacity of the queries that read the table.

## Look for

- A slow dashboard blamed on clustering lag, with a slot reservation left unchanged.
- Reclustering expected to finish a queued interactive query.
- A cluster spec added so a concurrency problem goes away.

## Fix shape

- Size slots and reservations for the query concurrency.
- Use clustering for block pruning, and partitioning when you also need a pre-run cost estimate.

## Field reports

No thread in the catalog communities stated this failure with a stack and a fix that matches the docs. Search [r/dataengineering](https://www.reddit.com/r/dataengineering/) and the tool tags in `sources/catalog.yaml` before treating a comment as a rule.

## Canonical check

- [Clustered tables](https://cloud.google.com/bigquery/docs/clustered-tables) — Automatic reclustering has no effect on query capacity. Clustering does not guarantee a reduction in the slots a query needs.

Reclustering is still worth doing so filters prune blocks. It is not a capacity setting.
