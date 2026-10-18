# Warehouse size

Warehouse size matches query complexity. Concurrency is handled by more clusters, not by a bigger warehouse.

## Look for

- An X-Large or larger warehouse used for UI, ad hoc metadata, and small transforms.
- A resize intended to clear a queue of many small queries.
- Mixed tiny and huge queries on one warehouse, so the size cannot be chosen.
- A running resize from 5XL or 6XL down to 4XL or smaller with no plan for the overlap charge.

## Fix shape

- Put similar queries on the same warehouse. Start from a size that finishes the typical query, and suspend it when idle.
- Add a multi-cluster warehouse in auto-scale mode for concurrency. Keep the minimum clusters at 1 unless a cluster failure must not stop the warehouse.
- Remember a resize does not speed up queries that have already started.

## Field reports

No thread in the catalog communities stated this failure with a stack and a fix that matches the docs. Search [r/dataengineering](https://www.reddit.com/r/dataengineering/) and the tool tags in `sources/catalog.yaml` before treating a comment as a rule.

## Canonical check

- [Warehouse considerations](https://docs.snowflake.com/en/user-guide/warehouses-considerations) — Credits scale with size and with the number of clusters. Larger is not faster for small queries. Resizing is not the tool for concurrency. Multi-cluster warehouses in auto-scale mode are.

Multi-cluster warehouses require Enterprise Edition or higher. An X-Large with 10 clusters bills for all 10 if they run the whole hour.
