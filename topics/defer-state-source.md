# Defer state source

Slim CI reads the manifest of the production job, not a busy staging project whose manifest changes under open pull requests.

## Look for

- `--state` or `DBT_ENGINE_STATE` pointed at a staging, QA, or UAT job that runs many different commands.
- A missing production artifact, so the selector builds the whole project or builds nothing useful.
- Ephemeral parents expected to defer. They never do.

## Fix shape

- Mark one production environment and use its last successful job as state.
- Pass the same manifest to `--state` and `--defer` unless you have a reason to split logical state from applied state.
- Rebuild CI when the production manifest moves and the pull request is behind.

## Field reports

No thread in the catalog communities stated this failure with a stack and a fix that matches the docs. Search [r/dataengineering](https://www.reddit.com/r/dataengineering/) and the tool tags in `sources/catalog.yaml` before treating a comment as a rule.

## Canonical check

- [dbt CI](https://docs.getdbt.com/guides/set-up-ci) — Point comparison at production. A busy non-production environment overwrites `manifest.json`.
- [Defer](https://docs.getdbt.com/reference/node-selection/defer) — Defer resolves an unselected parent from state only when it is not already in the CI database. Ephemeral models are never deferred.

State artifacts have to match the dbt version that is running. A manifest from an older minor can fail the comparison.
