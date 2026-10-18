# Iceberg schema evolution

Iceberg changes schema by column id. Adding, dropping, renaming, widening, or reordering does not rewrite data files.

## Look for

- A pipeline that creates a new table and copies every file because a column was renamed or reordered.
- A column dropped and then re-added under the same name, with readers that expect the old values to return. Iceberg assigns a new id, so old values stay with the old id.
- A type change that is not a widen.
- Partition layout changed by copying the table instead of evolving the spec.

## Fix shape

- Use Iceberg schema evolution for add, drop, rename, widen, and reorder.
- Evolve the partition spec in place when the layout must change. Old files keep their old layout. Plan splits across both.

## Field reports

No thread in the catalog communities stated this failure with a stack and a fix that matches the docs. Search [r/dataengineering](https://www.reddit.com/r/dataengineering/) and the tool tags in `sources/catalog.yaml` before treating a comment as a rule.

## Canonical check

- [Iceberg evolution](https://iceberg.apache.org/docs/latest/evolution/) — Schema updates are metadata changes. Added columns never read another column's values. Dropping or reordering a column does not change other values. Column identity is an id, not a name or a position.

Map keys do not support adding or dropping struct fields that would change equality. A name-based format can resurrect a dropped column when the name is reused. Iceberg does not.
