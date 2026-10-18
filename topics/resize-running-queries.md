# Resize and running queries

Additional warehouse resources apply to queued and new queries. Running queries keep the resources they started with.

## Look for

- An operator resizing from Small to Large because one query is slow, expecting that query to pick up the new nodes.
- A scale-down during a query, which drops cache with the removed resources.
- A resize used to clear a concurrency queue.

## Fix shape

- Let the running query finish or cancel it and rerun after the resize.
- Add clusters for concurrency. Resize for query complexity.

## Field reports

No thread in the catalog communities stated this failure with a stack and a fix that matches the docs. Search [r/dataengineering](https://www.reddit.com/r/dataengineering/) and the tool tags in `sources/catalog.yaml` before treating a comment as a rule.

## Canonical check

- [Warehouse considerations](https://docs.snowflake.com/en/user-guide/warehouses-considerations) — Resizing a running warehouse does not change queries already in flight. New resources serve queued and new queries. Scale-out is the concurrency tool.

Shrinking a 5XL or 6XL to 4XL or smaller bills both sizes briefly while the old warehouse quiesces.
