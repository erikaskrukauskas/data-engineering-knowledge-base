# Post-hook statistics

A Spark model that is planned from table statistics runs `ANALYZE TABLE` in a post-hook after the model is built.

## Look for

- A Spark or Databricks SQL model with no analyze step and a planner using empty stats.
- `ANALYZE` placed in a pre-hook, so it runs before the new data is written.
- The statement pointed at a hard-coded name instead of `{{ this }}`.

## Fix shape

- Put `analyze table {{ this }} compute statistics for all columns` in a post-hook.
- Prefer `{{ this }}` so the relation follows the target schema.

## Field reports

No thread in the catalog communities stated this failure with a stack and a fix that matches the docs. Search [r/dataengineering](https://www.reddit.com/r/dataengineering/) and the tool tags in `sources/catalog.yaml` before treating a comment as a rule.

## Canonical check

- [Hooks](https://docs.getdbt.com/reference/resource-configs/pre-hook-post-hook) — The Spark example is a post-hook that analyzes `{{ this }}` after creation. Hooks can be a list, and a macro can return the statement.

This is for the Spark adapter path the hook page shows. A warehouse that auto-collects statistics does not need the statement copied across.
