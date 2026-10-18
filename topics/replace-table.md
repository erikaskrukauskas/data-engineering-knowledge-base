# Replace a table

A full reload uses `CREATE OR REPLACE` or an overwrite that replaces the table atomically. It does not delete the directory and recreate it.

## Look for

- A job that deletes the table path and then writes a new table at that path.
- A Spark overwrite without `overwriteSchema` when the schema changed.
- Readers that fail or see a partial table during the delete.

## Fix shape

- Use `CREATE OR REPLACE TABLE` or a DataFrame overwrite with `overwriteSchema` when the schema is part of the change.
- Keep the path stable.

## Field reports

No thread in the catalog communities stated this failure with a stack and a fix that matches the docs. Search [r/dataengineering](https://www.reddit.com/r/dataengineering/) and the tool tags in `sources/catalog.yaml` before treating a comment as a rule.

## Canonical check

- [Delta best practices](https://docs.delta.io/best-practices/) — Do not delete the table directory and recreate it at the same path. That is not atomic. Replace the table with `CREATE OR REPLACE` or an overwrite.

`CREATE OR REPLACE` drops the old history. If you needed time travel to the previous version, overwrite is the wrong tool. Expire snapshots on a schedule instead of replacing the table.
