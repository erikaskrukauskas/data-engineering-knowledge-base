# Warehouse minimum charge

Each resume bills at least 60 seconds. A workload that resumes more often than that pays the minimum every time.

## Look for

- Auto-suspend of 1 minute and queries every 2 minutes.
- A task that resumes a warehouse, runs for 10 seconds, and suspends.
- A resize or a restart that overlaps the previous 60-second window.

## Fix shape

- Match auto-suspend to the real gap between queries.
- Leave the warehouse up through a burst that would otherwise resume twice.

## Field reports

No thread in the catalog communities stated this failure with a stack and a fix that matches the docs. Search [r/dataengineering](https://www.reddit.com/r/dataengineering/) and the tool tags in `sources/catalog.yaml` before treating a comment as a rule.

## Canonical check

- [Warehouse considerations](https://docs.snowflake.com/en/user-guide/warehouses-considerations) — The minimum charge is 60 seconds. A warehouse that runs 30 seconds is billed for 60. One that runs 61 seconds, stops, and runs again is billed for 121.

There is no refund for stopping inside the first minute. The credits are already billed.
