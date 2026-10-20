# One semantic model per dbt model

Each dbt model has at most one `semantic_model` block. A second grain gets its own model.

## Look for

- Two `semantic_model` blocks on one model.
- A fact and a dimension described in one semantic model because they share a SQL file.
- `semantic_model: false` left on a model that metrics still name.

## Fix shape

- Split grains into separate models.
- Set `semantic_model: true` or the mapping form on the model that metrics should use.
- Use `semantic_model.name` when the semantic name should differ from the model name.

## Field reports

No thread in the catalog communities stated this failure with a stack and a fix that matches the docs. Search [r/dataengineering](https://www.reddit.com/r/dataengineering/) and the tool tags in `sources/catalog.yaml` before treating a comment as a rule.

## Canonical check

- [Semantic models](https://docs.getdbt.com/docs/build/semantic-models) — Each dbt model defines one semantic model. The name defaults to the model name. `semantic_model` can be true, false, or a mapping.

The latest spec is v1.12 and later, plus the platform latest track and dbt v2. A legacy YAML file is a different shape.
