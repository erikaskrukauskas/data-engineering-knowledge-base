# Delta log retention

`delta.logRetentionDuration` is at least as long as the history you reconstruct. The default is 30 days.

## Look for

- Log retention shortened below the vacuum retention, so `VACUUM LITE` can no longer see the files.
- A compliance copy that expects a one-year log on the default 30 days.
- Log files deleted by a job. Delta deletes them after checkpoints.

## Fix shape

- Set `delta.logRetentionDuration` with a table property when 30 days is not the history you need.
- Keep it at least as long as the oldest version VACUUM LITE must understand.

## Field reports

No thread in the catalog communities stated this failure with a stack and a fix that matches the docs. Search [r/dataengineering](https://www.reddit.com/r/dataengineering/) and the tool tags in `sources/catalog.yaml` before treating a comment as a rule.

## Canonical check

- [Delta utilities](https://docs.delta.io/delta-utility/) — VACUUM deletes data files, not log files. Log retention defaults to 30 days via `delta.logRetentionDuration`. `VACUUM LITE` raises `DELTA_CANNOT_VACUUM_LITE` when the log it needs was pruned.

A longer log does not keep the data files. VACUUM still removes those after the file retention.
