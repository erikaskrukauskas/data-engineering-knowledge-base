# SQL unit test columns

`format: sql` supplies every column. Dict and csv fixtures may supply the columns the assertion uses.

## Look for

- A SQL fixture with two columns of a ten-column parent.
- Jinja `{{ var(...) }}` inside a SQL fixture.
- A csv fixture used where the parent is ephemeral.

## Fix shape

- Select every column in the SQL fixture.
- Remove Jinja from the fixture.
- Use dict or csv when only a subset of columns matters and the parent is not ephemeral.

## Field reports

No thread in the catalog communities stated this failure with a stack and a fix that matches the docs. Search [r/dataengineering](https://www.reddit.com/r/dataengineering/) and the tool tags in `sources/catalog.yaml` before treating a comment as a rule.

## Canonical check

- [Unit test data formats](https://docs.getdbt.com/reference/resource-properties/data-formats) — SQL format requires mock data for all columns. Jinja is unsupported in SQL fixtures. Dict and csv can list the relevant columns.

A csv fixture file lives under `tests/fixtures` or the configured test path. That path is for the fixture, not for the `unit_tests` block.
