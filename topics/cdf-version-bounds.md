# Change feed version bounds

`table_changes` includes the start version and the end version. A timestamp uses `yyyy-MM-dd[ HH:mm:ss[.SSS]]`.

## Look for

- An end version set to the last processed version, so that version is applied twice on the next run.
- A timestamp in a local format the function does not parse.
- A path-based table read with `table_changes` instead of `table_changes_by_path`.

## Fix shape

- Store the last version you applied and start the next read at the following version.
- Or set the end of this read and the start of the next so the inclusive bound is not duplicated.
- Use `table_changes_by_path` for a path-based table.

## Field reports

No thread in the catalog communities stated this failure with a stack and a fix that matches the docs. Search [r/dataengineering](https://www.reddit.com/r/dataengineering/) and the tool tags in `sources/catalog.yaml` before treating a comment as a rule.

## Canonical check

- [Change data feed](https://docs.delta.io/delta-change-data-feed/) — Start and end versions and timestamps are inclusive. The timestamp format is `yyyy-MM-dd[ HH:mm:ss[.SSS]]`. Path-based tables use `table_changes_by_path`.

Supplying only the start reads through the latest version. That window is still inclusive of the start.
