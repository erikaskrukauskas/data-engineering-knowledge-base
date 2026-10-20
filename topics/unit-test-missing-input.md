# Unit test inputs

A unit test lists every `ref` and `source` the model uses. A missing input is a node-not-found error at compile time.

## Look for

- A model that `ref`s two parents and a unit test that supplies one.
- A source read by the model with no `input: source(...)`.
- An empty parent represented by omitting the input instead of `rows: []`.

## Fix shape

- Add an `input` for every `ref` and `source`.
- Use `rows: []` when the parent should be empty.

## Field reports

No thread in the catalog communities stated this failure with a stack and a fix that matches the docs. Search [r/dataengineering](https://www.reddit.com/r/dataengineering/) and the tool tags in `sources/catalog.yaml` before treating a comment as a rule.

## Canonical check

- [Unit tests](https://docs.getdbt.com/docs/build/unit-tests) — Include every `ref` or `source` as an input or compilation reports node not found.
- [Unit test inputs](https://docs.getdbt.com/reference/resource-properties/unit-test-input) — An empty input is `rows: []`. A seed is used as-is unless the test supplies an input.

If you omit an input for a seed, dbt uses the seed. That is the exception. Models and sources are not.
