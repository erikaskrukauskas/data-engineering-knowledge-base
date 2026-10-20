# Unit tests stay in the project

Unit tests are added only to SQL models owned by the current project. Package models, Python models, materialized views, recursive SQL, and introspective queries are out of scope.

## Look for

- A unit test YAML aimed at a package model.
- A Python model with a `unit_tests` block.
- A materialized view unit test copied from a table model.

## Fix shape

- Test the package model in that package, or wrap it in a project model and test the wrapper.
- Keep unit tests on SQL models that are not materialized views, recursive, or introspective.

## Field reports

No thread in the catalog communities stated this failure with a stack and a fix that matches the docs. Search [r/dataengineering](https://www.reddit.com/r/dataengineering/) and the tool tags in `sources/catalog.yaml` before treating a comment as a rule.

## Canonical check

- [Unit tests](https://docs.getdbt.com/docs/build/unit-tests) — Unit tests support SQL models in the current project. They do not support materialized views, recursive SQL, introspective queries, or models outside the current project.

A seed input is allowed. The model under test is still a SQL model in the project.
