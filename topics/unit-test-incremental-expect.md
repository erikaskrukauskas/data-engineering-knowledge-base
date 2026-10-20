# Incremental unit test expectation

The `expect` block of an incremental unit test is the inserted or merged set. It is not a picture of `{{ this }}` after the materialization.

## Look for

- `overrides.macros.is_incremental: true` with `expect` equal to the full target table.
- No `input: this` while the model reads `{{ this }}` in incremental mode.
- `is_incremental` left true in the only test, so full-refresh behavior is never checked.

## Fix shape

- Supply `this` as an input when incremental mode reads it.
- Expect only the rows the statement will insert or merge.
- Add a second test with `is_incremental: false` for the full-refresh path.

## Field reports

No thread in the catalog communities stated this failure with a stack and a fix that matches the docs. Search [r/dataengineering](https://www.reddit.com/r/dataengineering/) and the tool tags in `sources/catalog.yaml` before treating a comment as a rule.

## Canonical check

- [Unit tests](https://docs.getdbt.com/docs/build/unit-tests) — For an incremental model, expected output is what will be merged or inserted, not the table after that write. There is no unit test for whether the framework then merged those rows correctly.

The incremental model still has to exist in the warehouse before the unit test, even if you built it with `--empty`. The unit test does not create it.
