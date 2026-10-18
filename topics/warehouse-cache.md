# Warehouse cache

Auto-suspend is weighed against the cache the next query will have to rebuild.

## Look for

- A warehouse suspended between two queries that read the same large tables.
- A size decrease that drops the cache of the removed nodes in the middle of the day.
- Auto-suspend disabled on a large warehouse that is idle overnight, to keep a cache nobody uses.

## Fix shape

- Keep a short auto-suspend when the cache is valuable across a few minutes.
- Suspend overnight when the morning rebuild is cheaper than idle credits.

## Field reports

No thread in the catalog communities stated this failure with a stack and a fix that matches the docs. Search [r/dataengineering](https://www.reddit.com/r/dataengineering/) and the tool tags in `sources/catalog.yaml` before treating a comment as a rule.

## Canonical check

- [Warehouse considerations](https://docs.snowflake.com/en/user-guide/warehouses-considerations) — The cache is dropped on suspend and when resources are removed by a scale-down. The next queries rebuild it.

Cache helps queries that read the same tables. It does not help a load of new files.
