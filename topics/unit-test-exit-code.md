# Unit test exit codes

A unit test exits 0 or 1. The row diff is in the log. A data test's failure count is the number of returned rows.

## Look for

- A CI script that treats a unit test failure count as the number of bad fixture rows.
- A unit test and a data test compared with the same threshold.
- `store_failures` expected to hold the unit test diff. The diff is the test output.

## Fix shape

- Alert on a non-zero unit test.
- Read the logged diff for which fixture row moved.
- Keep data-test failure counts for data tests.

## Field reports

No thread in the catalog communities stated this failure with a stack and a fix that matches the docs. Search [r/dataengineering](https://www.reddit.com/r/dataengineering/) and the tool tags in `sources/catalog.yaml` before treating a comment as a rule.

## Canonical check

- [Unit tests](https://docs.getdbt.com/docs/build/unit-tests) — Each unit test is one case. Results are 0 or 1, unlike data tests, which report one failure per returned row.

`dbt build` still runs unit tests in lineage order before the model and its data tests, unless the production job excluded the resource type.
