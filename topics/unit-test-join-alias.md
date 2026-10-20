# Unit test join aliases

A model with a join gives each input an alias before a unit test can exercise that join.

## Look for

- `from {{ ref('a') }} join {{ ref('b') }} using (id)` with no aliases.
- A unit test that fails or cannot bind columns because both inputs expose `id`.
- A select star across the join.

## Fix shape

- Alias each input in the model SQL.
- Project the columns the test needs from those aliases.

## Field reports

No thread in the catalog communities stated this failure with a stack and a fix that matches the docs. Search [r/dataengineering](https://www.reddit.com/r/dataengineering/) and the tool tags in `sources/catalog.yaml` before treating a comment as a rule.

## Canonical check

- [Unit tests](https://docs.getdbt.com/docs/build/unit-tests) — Table names must be aliased in order to unit test join logic.

The alias requirement is for the model under test. It does not replace listing each parent as an input.
