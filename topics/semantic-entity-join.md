# Semantic entity joins

MetricFlow joins semantic models on shared entity names. A foreign key uses the entity name of the primary key it points at.

## Look for

- `customer_id` on a fact named `customer_id` while the dimension names the entity `customer`.
- Two meanings of `customer` on unrelated tables.
- A join key expression left in the column name instead of `expr`.

## Fix shape

- Use one entity name for the same key.
- Set `expr` when the column name differs from the entity.
- Do not reuse an entity name for a different business key.

## Field reports

No thread in the catalog communities stated this failure with a stack and a fix that matches the docs. Search [r/dataengineering](https://www.reddit.com/r/dataengineering/) and the tool tags in `sources/catalog.yaml` before treating a comment as a rule.

## Canonical check

- [Semantic models](https://docs.getdbt.com/docs/build/semantic-models) — Entity names can repeat across semantic models because MetricFlow uses them as join keys. Names must be unique inside one semantic model.

A repeated entity name is a join. It is not a dimension collision. Dimensions on the same primary entity still need unique names.
