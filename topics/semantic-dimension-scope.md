# Dimension name scope

Two semantic models with the same primary entity do not define the same dimension name.

## Look for

- `user__country` defined on both a users model and a user-attributes model that share the `user` entity.
- A categorical dimension and a time dimension given the same name.
- A query that groups by the short name and can hit either definition.

## Fix shape

- Rename one dimension.
- Repeat a dimension name only when the primary entities differ. Reference it as `entity__dimension`.

## Field reports

No thread in the catalog communities stated this failure with a stack and a fix that matches the docs. Search [r/dataengineering](https://www.reddit.com/r/dataengineering/) and the tool tags in `sources/catalog.yaml` before treating a comment as a rule.

## Canonical check

- [Semantic models](https://docs.getdbt.com/docs/build/semantic-models) — Dimension names must be unique among semantic models with the same primary entity. The qualified name is `entity__dimension`.

Entity names are allowed to repeat across semantic models. That repeat is how MetricFlow joins. Dimension names are the ones that must not collide on one entity.
