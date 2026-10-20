# Unit test file path

Unit test YAML lives in `models/`, or another `model-paths` directory. The `tests/` directory is for data tests.

## Look for

- `unit_tests:` inside `tests/schema.yml`.
- A unit test file that dbt never discovers because it sits outside `model-paths`.
- A data test and a unit test mixed in `tests/` and only the data test runs.

## Fix shape

- Move `unit_tests` into a YAML file under `models/`.
- Keep singular and generic data tests in `tests/`.

## Field reports

No thread in the catalog communities stated this failure with a stack and a fix that matches the docs. Search [r/dataengineering](https://www.reddit.com/r/dataengineering/) and the tool tags in `sources/catalog.yaml` before treating a comment as a rule.

## Canonical check

- [Unit tests](https://docs.getdbt.com/docs/build/unit-tests) — Unit tests are discovered from `model-paths`. They must not be defined in `tests/`, which is reserved for data tests.

Fixtures for csv or sql format can live under a `fixtures` directory on a test path, such as `tests/fixtures`. The unit test definition itself stays with the models.
