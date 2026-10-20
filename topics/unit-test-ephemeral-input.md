# Ephemeral unit test input

A unit test input for an ephemeral model uses `format: sql`.

## Look for

- `input: ref('ephemeral_model')` with `format: dict` or `format: csv`.
- An ephemeral model unit-tested on its own. The page's pattern is the downstream model.
- A sql fixture that contains Jinja.

## Fix shape

- Set `format: sql` and provide the rows as a select.
- Keep Jinja out of the fixture. Jinja is unsupported in SQL fixtures.

## Field reports

No thread in the catalog communities stated this failure with a stack and a fix that matches the docs. Search [r/dataengineering](https://www.reddit.com/r/dataengineering/) and the tool tags in `sources/catalog.yaml` before treating a comment as a rule.

## Canonical check

- [Unit tests](https://docs.getdbt.com/docs/build/unit-tests) — An ephemeral parent must be provided as `format: sql`.
- [Unit test data formats](https://docs.getdbt.com/reference/resource-properties/data-formats) — `format: sql` requires mock data for all columns. Jinja is unsupported in SQL fixtures.

Dict and csv inputs may list only the columns the test cares about. SQL inputs may not. They must include every column.
