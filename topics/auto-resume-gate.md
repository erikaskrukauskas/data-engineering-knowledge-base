# Auto-resume gate

Auto-resume is on for a warehouse that jobs must start themselves. It is off only when a person is the gate.

## Look for

- Auto-resume disabled on the transform warehouse and a DAG that submits SQL without a resume call.
- Auto-resume enabled on a warehouse shared with ad hoc users, as a cost control that does not control anything.
- A resume that bills 60 seconds for a metadata query that did not need a warehouse.

## Fix shape

- Enable auto-resume on job warehouses, and auto-suspend so they stop.
- Disable auto-resume only where a human should decide the warehouse runs.
- Point UI metadata queries at a small warehouse, or at statements that do not need one.

## Field reports

No thread in the catalog communities stated this failure with a stack and a fix that matches the docs. Search [r/dataengineering](https://www.reddit.com/r/dataengineering/) and the tool tags in `sources/catalog.yaml` before treating a comment as a rule.

## Canonical check

- [Warehouse considerations](https://docs.snowflake.com/en/user-guide/warehouses-considerations) — Auto-resume starts the warehouse when a query arrives. Leave it off to force a manual resume. Some metadata statements do not use a warehouse and are not billed.

Enabling auto-resume without auto-suspend is how a warehouse stays up. Set both.
