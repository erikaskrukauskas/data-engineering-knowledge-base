# Semantic model names

A semantic model name avoids `__`. Dimension references already use `__` between the entity and the dimension.

## Look for

- `semantic_model.name` or a model name containing `__`.
- A filter written as `entity__dimension` that collides with a model name that already contains `__`.

## Fix shape

- Rename the semantic model so the name has single underscores only.
- Keep `entity__dimension` for filters, which is the qualified dimension form.

## Field reports

No thread in the catalog communities stated this failure with a stack and a fix that matches the docs. Search [r/dataengineering](https://www.reddit.com/r/dataengineering/) and the tool tags in `sources/catalog.yaml` before treating a comment as a rule.

## Canonical check

- [Semantic models](https://docs.getdbt.com/docs/build/semantic-models) — Double underscores in the semantic model name are not supported. A dimension is referenced as `entity__dimension_name`.

The model name can differ from the semantic model name. Set `semantic_model.name` when the relation name is not a legal semantic name.
