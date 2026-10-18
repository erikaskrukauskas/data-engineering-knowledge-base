# Staging source calls

Staging is the only layer that calls `source()`. Marts call `ref()`.

## Look for

- `source()` inside a mart or an intermediate model.
- A staging model that joins two sources.
- A staging model that filters to a business subset so later models cannot see the rest of the entity.

## Fix shape

- One staging model per source relation. Rename and cast there.
- Join in intermediate models. Aggregate in marts.

## Field reports

No thread in the catalog communities stated this failure with a stack and a fix that matches the docs. Search [r/dataengineering](https://www.reddit.com/r/dataengineering/) and the tool tags in `sources/catalog.yaml` before treating a comment as a rule.

## Canonical check

- [Staging](https://docs.getdbt.com/best-practices/how-we-structure/2-staging) — Staging is 1:1 with a source relation. It is where `source()` belongs. Joins and aggregations do not.
- [Intermediate](https://docs.getdbt.com/best-practices/how-we-structure/3-intermediate) — Intermediate models are where joins that are not the final mart belong.
- [Marts](https://docs.getdbt.com/best-practices/how-we-structure/4-marts) — Marts are the entity tables consumers query.

dbt allows a unit conversion in staging when every downstream model needs that unit. A conversion that drops precision stays out of staging.
