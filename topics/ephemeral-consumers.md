# Ephemeral consumers

Ephemeral models are compiled into their parent. They have no relation a dashboard can query, and they cannot be deferred.

## Look for

- `materialized='ephemeral'` on a mart named in an exposure.
- A CI job that expects an ephemeral parent to come from production via `--defer`.
- An intermediate model materialized as ephemeral and then selected by an analyst.

## Fix shape

- Materialize marts as tables or incremental models.
- Leave ephemeral for a transformation that is only ever referenced by later models in the same project.

## Field reports

No thread in the catalog communities stated this failure with a stack and a fix that matches the docs. Search [r/dataengineering](https://www.reddit.com/r/dataengineering/) and the tool tags in `sources/catalog.yaml` before treating a comment as a rule.

## Canonical check

- [Defer](https://docs.getdbt.com/reference/node-selection/defer) — Ephemeral models are never deferred.
- [Marts](https://docs.getdbt.com/best-practices/how-we-structure/4-marts) — Marts are the relations consumers query.

Changing an ephemeral model to a view creates a relation that grants and schema names now apply to. Set those in the same change.
