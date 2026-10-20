# Change data folder

Insert-only writes and full partition deletes may leave `_change_data` empty because Delta computes them from the log.

## Look for

- A monitor that alerts when `_change_data` has no files after an insert.
- A job that deletes the folder.
- A reader that lists the folder instead of calling `table_changes` or `readChangeFeed`.

## Fix shape

- Read through `table_changes` or `readChangeFeed`.
- Treat a missing folder as normal for insert-only commits.

## Field reports

No thread in the catalog communities stated this failure with a stack and a fix that matches the docs. Search [r/dataengineering](https://www.reddit.com/r/dataengineering/) and the tool tags in `sources/catalog.yaml` before treating a comment as a rule.

## Canonical check

- [Change data feed](https://docs.delta.io/delta-change-data-feed/) — UPDATE, DELETE, and MERGE can write `_change_data`. Insert-only operations and full partition deletes may not, because the log is enough.

VACUUM can also remove the folder's files after retention. An empty folder after vacuum is the retention note, not this one.
