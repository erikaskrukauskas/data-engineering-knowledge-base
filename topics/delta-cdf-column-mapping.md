# Change feed and column mapping

With column mapping, a CDF read fails when the version range crosses a rename, drop, type change, or nullability change.

## Look for

- `startingVersion` and `endingVersion` that include a column rename.
- A streaming CDF read on Delta 2.1 or 2.2 with column mapping. Streaming reads are unsupported there until 2.3, and 2.3 still cannot span the non-additive change.
- A batch read on Delta 2.0 or earlier with column mapping.

## Fix shape

- Read up to the version before the schema change, then start again after it.
- On Delta 3.0 and later, enable schema tracking before streaming across a non-additive change.
- Check the Delta version before promising CDF on a column-mapped table.

## Field reports

No thread in the catalog communities stated this failure with a stack and a fix that matches the docs. Search [r/dataengineering](https://www.reddit.com/r/dataengineering/) and the tool tags in `sources/catalog.yaml` before treating a comment as a rule.

## Canonical check

- [Change data feed](https://docs.delta.io/delta-change-data-feed/) — Column mapping limits CDF. Before 2.3, non-additive changes block the supported reads. From 2.3, batch reads use the end version's schema and still fail if the range spans the change. Streaming across that change needs Delta 3.0 schema tracking.

Additive changes are the supported case. A new column is not the failure this note is about.
