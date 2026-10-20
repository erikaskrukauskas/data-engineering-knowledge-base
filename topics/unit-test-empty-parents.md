# Empty parents for unit tests

The direct parents of a unit-tested model exist in the warehouse. `--empty` builds them without copying data.

## Look for

- A unit test run in a fresh CI schema before the parents exist.
- A full `dbt build` of large parents only to unlock one unit test.
- An incremental parent that was never created, even empty.

## Fix shape

- Run `dbt run --select <parents> --empty` before the unit test.
- Or `dbt build`, which runs the unit test, materializes the model, then runs data tests, in lineage order.

## Field reports

No thread in the catalog communities stated this failure with a stack and a fix that matches the docs. Search [r/dataengineering](https://www.reddit.com/r/dataengineering/) and the tool tags in `sources/catalog.yaml` before treating a comment as a rule.

## Canonical check

- [Unit tests](https://docs.getdbt.com/docs/build/unit-tests) — Direct parents must exist before the unit test executes. `--empty` builds them without a full data load. Incremental models need the same empty build.

`--empty` creates the relation. It does not satisfy a unit test input. The fixture still has to be declared.
