# dbt clean

`dbt clean` removes local artifacts such as `target/`. Warehouse tables remain until something drops them.

## Look for

- `dbt clean` in a deploy job described as the way deprecated models are removed.
- A CI schema that survives because clean ran and drop did not.
- A snapshot schema included in a warehouse drop list.

## Fix shape

- Drop orphaned relations with a query against the information schema, excluding the snapshot schema.
- Let the pull-request job drop its own CI schema.

## Field reports

No thread in the catalog communities stated this failure with a stack and a fix that matches the docs. Search [r/dataengineering](https://www.reddit.com/r/dataengineering/) and the tool tags in `sources/catalog.yaml` before treating a comment as a rule.

## Canonical check

- [Custom schemas](https://docs.getdbt.com/docs/build/custom-schemas) — Relations may live outside `target.schema`. A drop list has to include those schemas.
- [dbt CI](https://docs.getdbt.com/guides/set-up-ci) — The platform drops the pull-request schema when the pull request is closed. `dbt clean` is not that step.

`dbt clean` removes local project artifacts. Dropping a warehouse relation is a separate statement, and it has to list every custom schema the project writes.
