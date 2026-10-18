# Warehouse auto-suspend

Auto-suspend is a few minutes, matched to the real gap between queries, and auto-resume is on for warehouses people must be able to use.

## Look for

- `AUTO_SUSPEND = 0` or Never on a warehouse that is idle most of the hour.
- Auto-suspend of 1 minute when queries arrive every 2 minutes, so the warehouse pays the 60-second minimum on every resume.
- Auto-resume left off on a warehouse that transforms are expected to start themselves.

## Fix shape

- Set auto-suspend to 5 or 10 minutes or less, unless the workload has a steady gap that is shorter than that and you are paying the resume minimum.
- Enable auto-resume when cost control is the suspend setting, not a manual gate.
- Disable auto-suspend only for a heavy steady warehouse, and record the credit cost of leaving it up.

## Field reports

No thread in the catalog communities stated this failure with a stack and a fix that matches the docs. Search [r/dataengineering](https://www.reddit.com/r/dataengineering/) and the tool tags in `sources/catalog.yaml` before treating a comment as a rule.

## Canonical check

- [Warehouse considerations](https://docs.snowflake.com/en/user-guide/warehouses-considerations) — Snowflake bills a 60-second minimum each time compute is provisioned, then per second. Suspending and resuming inside that minute costs more than leaving it up. The cache is dropped on suspend.

The first 60 seconds are billed even if the warehouse ran for 30. Stopping early does not refund them. A resize down also drops the cache of the removed resources.
