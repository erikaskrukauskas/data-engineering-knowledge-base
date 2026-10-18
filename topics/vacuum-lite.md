# Vacuum lite

`VACUUM LITE` is the faster mode when the log still lists the files. A pruned log means a full vacuum.

## Look for

- `VACUUM LITE` on a table whose log retention was cut to a few days.
- A full `VACUUM` on a huge table that only needed the log-based pass.
- A retention under 7 days passed to either mode.

## Fix shape

- Prefer `VACUUM LITE` when the log is intact.
- Fall back to `VACUUM` without `LITE` after `DELTA_CANNOT_VACUUM_LITE`.
- Keep retention at or above 7 days.

## Field reports

No thread in the catalog communities stated this failure with a stack and a fix that matches the docs. Search [r/dataengineering](https://www.reddit.com/r/dataengineering/) and the tool tags in `sources/catalog.yaml` before treating a comment as a rule.

## Canonical check

- [Delta utilities](https://docs.delta.io/delta-utility/) — `VACUUM LITE` uses the transaction log instead of listing the directory. It fails with `DELTA_CANNOT_VACUUM_LITE` if that log was pruned.

LITE still deletes files outside the retention window. It is faster, not safer to point at a short retention.
