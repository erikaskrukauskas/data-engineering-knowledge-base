# Versioned model unit tests

When a model has multiple versions, the unit test runs on all of them by default.

## Look for

- One unit test added while a v1 and v2 model both exist, and v1 fails the new expectation.
- A version pin missing from `input: ref('model', v='2')` when only one version should be mocked.
- A production exclusion that still leaves the unit test running on every version in CI without a version selector.

## Fix shape

- Read the versioned-model unit test note before adding the test.
- Point inputs at the version the test is about.

## Field reports

No thread in the catalog communities stated this failure with a stack and a fix that matches the docs. Search [r/dataengineering](https://www.reddit.com/r/dataengineering/) and the tool tags in `sources/catalog.yaml` before treating a comment as a rule.

## Canonical check

- [Unit tests](https://docs.getdbt.com/docs/build/unit-tests) — By default a unit test runs on all versions of the model. Unit tests can only be added to models in the current project, and only to SQL models.

Package models cannot take a unit test from the root project. The test has to live with the project that owns the model.
