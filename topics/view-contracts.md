# View contracts

A contracted view enforces column names and data types. It does not enforce constraints.

## Look for

- `not_null` or `unique` constraints on a view, expected to reject rows.
- A view contracted and then selected as if the database guaranteed the grain.
- A contract on an ephemeral model.

## Fix shape

- Contract the view for names and types.
- Put `unique` and `not_null` tests on the model that materializes the rows.

## Field reports

No thread in the catalog communities stated this failure with a stack and a fix that matches the docs. Search [r/dataengineering](https://www.reddit.com/r/dataengineering/) and the tool tags in `sources/catalog.yaml` before treating a comment as a rule.

## Canonical check

- [Model contracts](https://docs.getdbt.com/docs/mesh/govern/model-contracts) — Views support names and types, not constraints. Contracts are for models that are tables, incremental models, or views, and not for ephemeral models, snapshots, seeds, or sources.

An incremental contract also needs `on_schema_change` of `append_new_columns` or `fail`. `ignore` is not one of those two.
