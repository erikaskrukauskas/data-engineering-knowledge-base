# Mart grain

A mart names its grain and does not mix two grains in one model.

## Look for

- An orders mart with one row per order line and also one row per order.
- A mart that joins a fact to a fact and multiplies rows.
- A description that does not state the grain.

## Fix shape

- One mart per entity. Put the grain in the model description.
- Build intermediate models for the joins that are not the final grain.

## Field reports

No thread in the catalog communities stated this failure with a stack and a fix that matches the docs. Search [r/dataengineering](https://www.reddit.com/r/dataengineering/) and the tool tags in `sources/catalog.yaml` before treating a comment as a rule.

## Canonical check

- [Marts](https://docs.getdbt.com/best-practices/how-we-structure/4-marts) — Marts are organized by entity. A mart is the table a consumer queries, wide when there is no semantic layer.
- [dbt documentation](https://docs.getdbt.com/docs/build/documentation) — The description is where the grain is written next to the tests that check it.

A wide mart is what the dbt mart page recommends when you are not using the semantic layer. Wide is not the same as two grains.
