# State modified selector

A pull-request job runs `dbt build --select state:modified+` against the production manifest.

## Look for

- A CI job that runs `dbt build` with no selector.
- `state:modified` without the `+`, so downstream models are not built.
- `--full-refresh` added to that selector.
- State taken from a non-production job.

## Fix shape

- Select `state:modified+` and defer upstream to production.
- Compare with the production manifest.

## Field reports

No thread in the catalog communities stated this failure with a stack and a fix that matches the docs. Search [r/dataengineering](https://www.reddit.com/r/dataengineering/) and the tool tags in `sources/catalog.yaml` before treating a comment as a rule.

## Canonical check

- [dbt CI](https://docs.getdbt.com/guides/set-up-ci) — The pull-request job builds `state:modified+` and does not full-refresh. State comes from production.

A comment on a CI thread recommends adding `--full-refresh` to slim CI. The CI guide does not. Keep the disagreement in mind when someone copies that comment.
