# Persisted column docs

`persist_docs` is enabled when the warehouse comment should match the YAML description.

## Look for

- Descriptions in YAML and empty comments in the warehouse, with consumers who never open dbt docs.
- `persist_docs` enabled for columns on a warehouse that does not support column comments.
- A contract and a description that disagree about a column's meaning.

## Fix shape

- Set `persist_docs` for relations, and for columns where the adapter supports it.
- Keep the YAML description as the text. The warehouse comment is a copy.

## Field reports

No thread in the catalog communities stated this failure with a stack and a fix that matches the docs. Search [r/dataengineering](https://www.reddit.com/r/dataengineering/) and the tool tags in `sources/catalog.yaml` before treating a comment as a rule.

## Canonical check

- [dbt documentation](https://docs.getdbt.com/docs/build/documentation) — Descriptions live next to the model. `persist_docs` writes them to the database when you turn it on. Catalog columns still appear even when a column has no description.

A persisted comment does not enforce a type. The contract does that, on models that support contracts.
