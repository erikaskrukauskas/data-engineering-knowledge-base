# Column identity

Iceberg identifies a column by id. Reusing a name creates a new column. Old values stay with the old id.

## Look for

- A column dropped and re-added to undo the drop, with readers expecting the old values.
- A Hive-style job that matches columns by name or position and then reads an Iceberg table the same way.
- A rename done by adding a column and dropping the old one, which loses the lineage of the values unless you copy them.

## Fix shape

- Rename with schema evolution so the id stays.
- Add a column only when it is a new attribute.
- Do not reuse a dropped name for a different meaning without telling consumers the id changed.

## Field reports

No thread in the catalog communities stated this failure with a stack and a fix that matches the docs. Search [r/dataengineering](https://www.reddit.com/r/dataengineering/) and the tool tags in `sources/catalog.yaml` before treating a comment as a rule.

## Canonical check

- [Iceberg evolution](https://iceberg.apache.org/docs/latest/evolution/) — Column identity is an id. A dropped column that is re-added gets a new id. Name-based formats can resurrect values. Iceberg does not.

This is the Iceberg rule. A Hive or plain Parquet table can still bind by name. Do not apply the id rule there.
