# Semantic primary entity

A semantic model has one primary entity, either on a column or as `primary_entity`.

## Look for

- Dimensions defined with no column `entity.type: primary` and no top-level `primary_entity`.
- A fact marked `type: unique` when the table holds every record of that entity. Unique allows a subset and nulls.
- A foreign key marked primary, so MetricFlow treats a repeating key as the model's identity.

## Fix shape

- Mark the real key `type: primary`, or set `primary_entity` when the key is not a column.
- Use `foreign` for keys that repeat. Use `unique` only when the table is a subset.

## Field reports

No thread in the catalog communities stated this failure with a stack and a fix that matches the docs. Search [r/dataengineering](https://www.reddit.com/r/dataengineering/) and the tool tags in `sources/catalog.yaml` before treating a comment as a rule.

## Canonical check

- [Semantic models](https://docs.getdbt.com/docs/build/semantic-models) — Dimensions must be tied to an entity. Primary means one row per record for the whole platform. Unique may be a subset and may contain nulls. Foreign may repeat.

Assigning `primary_entity` names the entity. It does not have to be a physical column, and it does not change the SQL that builds the model.
