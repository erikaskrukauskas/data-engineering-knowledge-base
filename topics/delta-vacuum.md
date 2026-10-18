# Delta vacuum

VACUUM runs on a schedule with a retention at least as long as the longest reader and the longest stream lag.

## Look for

- `VACUUM ... RETAIN` below 7 days, or `retentionDurationCheck.enabled` set to false.
- No VACUUM at all after repeated `OPTIMIZE`, `DELETE`, or `MERGE`, so old files accumulate.
- `VACUUM LITE` on a table whose log was already pruned.
- A stream or a time-travel query that needs versions older than the vacuum retention.

## Fix shape

- Keep the default 7-day file retention unless the longest concurrent transaction and stream lag are known and shorter.
- Run `VACUUM ... DRY RUN` before a shorter retention.
- Set `delta.logRetentionDuration` separately when the log must outlive the data files.

## Field reports

No thread in the catalog communities stated this failure with a stack and a fix that matches the docs. Search [r/dataengineering](https://www.reddit.com/r/dataengineering/) and the tool tags in `sources/catalog.yaml` before treating a comment as a rule.

## Canonical check

- [Delta utilities](https://docs.delta.io/delta-utility/) — VACUUM is not automatic. The default file retention is 7 days. Time travel older than that is gone after VACUUM. A shorter interval can delete files a concurrent reader has not committed, which can corrupt the table.

VACUUM deletes data files, not log files. Log files default to 30 days via `delta.logRetentionDuration`. `VACUUM LITE` fails with `DELTA_CANNOT_VACUUM_LITE` if the log it needs was pruned.
