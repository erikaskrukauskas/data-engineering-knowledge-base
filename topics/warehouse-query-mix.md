# Warehouse query mix

A warehouse runs queries of similar size so its size can be chosen.

## Look for

- Dashboards, loads, and one-off backfills on the same warehouse.
- A size chosen for the largest query and left there for the small ones.
- No separate warehouse for the UI.

## Fix shape

- Split load, transform, and interactive queries.
- Size each warehouse for the queries it actually runs, and suspend it.

## Field reports

No thread in the catalog communities stated this failure with a stack and a fix that matches the docs. Search [r/dataengineering](https://www.reddit.com/r/dataengineering/) and the tool tags in `sources/catalog.yaml` before treating a comment as a rule.

## Canonical check

- [Warehouse considerations](https://docs.snowflake.com/en/user-guide/warehouses-considerations) — Mixed complexity makes it harder to pick a size. Homogeneous queries on one warehouse are the recommendation. The UI wants its own small warehouse when the account is busy.

A dedicated warehouse that never suspends costs more than the mix did. Set auto-suspend on each of them.
