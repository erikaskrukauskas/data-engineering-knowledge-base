# Derived semantic expressions

`derived_semantics` is how a dimension or entity is defined when it is not one physical column, and `expr` is required.

## Look for

- A derived dimension with a name and no `expr`.
- A SQL expression stuffed into the column `name` instead of `expr`.
- A join key computed with `substring` or a concatenation that is not declared as a derived entity.

## Fix shape

- Put the SQL in `expr` under `derived_semantics`.
- Keep a physical column's entity on that column when the join key is the column itself.

## Field reports

No thread in the catalog communities stated this failure with a stack and a fix that matches the docs. Search [r/dataengineering](https://www.reddit.com/r/dataengineering/) and the tool tags in `sources/catalog.yaml` before treating a comment as a rule.

## Canonical check

- [Semantic models](https://docs.getdbt.com/docs/build/semantic-models) — `derived_semantics` is for dimensions and entities that are not a single physical column. `expr` is required.

An `expr` on a normal entity block is for the case where the join key differs from the column. `derived_semantics` is the block for a key that has no single column.
