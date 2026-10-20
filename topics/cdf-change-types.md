# Change feed row images

A CDF consumer handles `_change_type` values `insert`, `delete`, `update_preimage`, and `update_postimage`.

## Look for

- A merge that treats every CDF row as an insert.
- `update_preimage` loaded as a new version of the entity.
- `_commit_version` ignored, so the same key from two versions collapses.

## Fix shape

- Apply deletes and preimages as the before-row.
- Apply inserts and postimages as the after-row.
- Keep `_commit_version` when more than one version is read.

## Field reports

No thread in the catalog communities stated this failure with a stack and a fix that matches the docs. Search [r/dataengineering](https://www.reddit.com/r/dataengineering/) and the tool tags in `sources/catalog.yaml` before treating a comment as a rule.

## Canonical check

- [Change data feed](https://docs.delta.io/delta-change-data-feed/) — The feed adds `_change_type`, `_commit_version`, and `_commit_timestamp`. An update is a preimage and a postimage. The schema of the read is the latest table version, except for the column-mapping cases.

Insert-only operations and full partition deletes may not appear as files in `_change_data`. The read API still returns the changes Delta can compute from the log.
