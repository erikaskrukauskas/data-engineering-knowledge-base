# Public model contracts

A model with `access: public` that other work selects has an enforced contract.

## Look for

- `access: public` with no `contract.enforced`.
- A contract on a staging model that is still changing shape.
- A public incremental model whose `on_schema_change` is `ignore` or `sync_all_columns`. Contracted incremental models allow `append_new_columns` or `fail`.

## Fix shape

- Enforce the contract on the public mart: every column name and data type.
- Keep staging private and uncontracted until the shape is stable.

## Field reports

No thread in the catalog communities stated this failure with a stack and a fix that matches the docs. Search [r/dataengineering](https://www.reddit.com/r/dataengineering/) and the tool tags in `sources/catalog.yaml` before treating a comment as a rule.

## Canonical check

- [Model contracts](https://docs.getdbt.com/docs/mesh/govern/model-contracts) — dbt recommends contracts for public models other teams or tools query. Incremental models must use `on_schema_change` of `append_new_columns` or `fail`.
- [Model access](https://docs.getdbt.com/docs/mesh/govern/model-access) — Public means any group or project may `ref` the model. Apply the contract before that promise.

Contracts do not apply to snapshots, seeds, sources, or Python models. A public snapshot is still not a contracted model.
