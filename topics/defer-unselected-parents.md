# Deferred parents

`--defer` fills an unselected parent from state. A parent that CI builds comes from the CI schema.

## Look for

- `--defer` without `--state`.
- An expectation that a modified parent is skipped because production has a copy.
- An ephemeral parent named in a defer failure.
- Two different manifests passed to `--state` and `--defer` without a reason.

## Fix shape

- Pass one production manifest to both flags.
- Read the defer error. If the parent is modified, CI is supposed to build it.

## Field reports

No thread in the catalog communities stated this failure with a stack and a fix that matches the docs. Search [r/dataengineering](https://www.reddit.com/r/dataengineering/) and the tool tags in `sources/catalog.yaml` before treating a comment as a rule.

## Canonical check

- [Defer](https://docs.getdbt.com/reference/node-selection/defer) — Defer applies to unselected parents that are not already in the warehouse schema. Ephemeral models are never deferred.

Defer does not clone production data. It points at the production relation. A CI model that reads it sees production rows.
