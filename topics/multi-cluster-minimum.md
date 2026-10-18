# Multi-cluster minimum

The minimum cluster count stays 1 unless a cluster failure must not stop the warehouse. The mode is auto-scale.

## Look for

- Minimum and maximum both set to the same high number, which is maximized mode.
- Minimum above 1 with no availability requirement.
- A single-cluster warehouse resized to clear a queue of many small queries.

## Fix shape

- Use auto-scale. Keep the minimum at 1. Set the maximum to what you can pay if every cluster runs the whole hour.
- Resize only when each query needs more resources.

## Field reports

No thread in the catalog communities stated this failure with a stack and a fix that matches the docs. Search [r/dataengineering](https://www.reddit.com/r/dataengineering/) and the tool tags in `sources/catalog.yaml` before treating a comment as a rule.

## Canonical check

- [Warehouse considerations](https://docs.snowflake.com/en/user-guide/warehouses-considerations) — Auto-scale is the default recommendation. Minimum 1 starts clusters when needed. A higher minimum is for availability. Multi-cluster requires Enterprise Edition or higher.

An X-Large with a maximum of 10 can bill 160 credits in an hour if all clusters run. The maximum is a spend cap only when clusters are not all busy.
