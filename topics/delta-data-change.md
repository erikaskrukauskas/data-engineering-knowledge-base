# Delta dataChange

A write that changes rows leaves `dataChange` at its default. Compaction sets it false.

## Look for

- `dataChange=false` on a `MERGE`, `UPDATE`, or `DELETE`.
- A compaction that sets `dataChange=true` and makes downstream streams reread every compacted row as new.
- A table corrupted by a false flag that skipped change tracking.

## Fix shape

- Set `dataChange=false` only for an operation that rewrites files without changing rows, such as compaction.
- Leave it true for every change consumers must see.

## Field reports

No thread in the catalog communities stated this failure with a stack and a fix that matches the docs. Search [r/dataengineering](https://www.reddit.com/r/dataengineering/) and the tool tags in `sources/catalog.yaml` before treating a comment as a rule.

## Canonical check

- [Delta best practices](https://docs.delta.io/best-practices/) — Setting `dataChange=false` on a write that changes rows can corrupt the table. Compaction uses false because the rows are the same.

Streams and changed-data feeds trust this flag. A wrong value is not a performance tweak. It drops or invents changes.
