# Unit test macro overrides

Overrides in a unit test set macros, vars, and environment variables so each branch of the model SQL can be exercised.

## Look for

- An incremental model tested only with the default `is_incremental()` result.
- A `var('backfill')` branch with no override, so the test always takes the other branch.
- One unit test expected to cover both modes.

## Fix shape

- Add a test per branch.
- Set `overrides.macros`, vars, or environment variables to the value that selects that branch.

## Field reports

No thread in the catalog communities stated this failure with a stack and a fix that matches the docs. Search [r/dataengineering](https://www.reddit.com/r/dataengineering/) and the tool tags in `sources/catalog.yaml` before treating a comment as a rule.

## Canonical check

- [Unit tests](https://docs.getdbt.com/docs/build/unit-tests) — Overrides change macros, vars, and environment variables. The incremental example sets `is_incremental` false in one test and true in another.

The override does not check that dbt then merges the expected rows into the table. It checks the statement the model returns in that mode.
