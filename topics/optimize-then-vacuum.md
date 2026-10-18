# Optimize then vacuum

Compaction rewrites files. VACUUM is the command that deletes the old ones, after retention.

## Look for

- `OPTIMIZE` on a schedule and no `VACUUM`, so storage grows.
- `VACUUM` immediately after `OPTIMIZE` with a retention under 7 days.
- `VACUUM` expected to compact small files. It does not.

## Fix shape

- Compact, then vacuum on a later schedule with retention of at least 7 days.
- Dry-run a shorter retention before using it.

## Field reports

No thread in the catalog communities stated this failure with a stack and a fix that matches the docs. Search [r/dataengineering](https://www.reddit.com/r/dataengineering/) and the tool tags in `sources/catalog.yaml` before treating a comment as a rule.

## Canonical check

- [Delta utilities](https://docs.delta.io/delta-utility/) — VACUUM removes files older than the retention threshold. It is not automatic. The default threshold is 7 days.
- [Delta best practices](https://docs.delta.io/best-practices/) — Compaction is a separate step from removing the files it replaced.

Time travel to a version older than the retention is gone after VACUUM. Do not vacuum a table whose readers still need that version.
