# Iceberg partition evolution

Iceberg partition evolution is a metadata change. Old files keep the layout they were written with.

## Look for

- A job that copies the whole table to change a partition column.
- A query that assumes every file is partitioned the new way and prunes nothing on old files.
- A Hive-style `ALTER TABLE ADD PARTITION` against an Iceberg table.

## Fix shape

- Evolve the spec. Write new data with the new spec.
- Plan queries so filters still match files written under the old spec.

## Field reports

No thread in the catalog communities stated this failure with a stack and a fix that matches the docs. Search [r/dataengineering](https://www.reddit.com/r/dataengineering/) and the tool tags in `sources/catalog.yaml` before treating a comment as a rule.

## Canonical check

- [Iceberg evolution](https://iceberg.apache.org/docs/latest/evolution/) — Partition evolution is metadata. Existing data files are not rewritten. New data uses the new spec.

A filter on the new partition column does not prune files that were written without that partition. Those files are still read.
