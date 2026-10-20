# Stream schema changes

A stream reads historic rows with the current schema. An incompatible change between the offset and the consume fails the query.

## Look for

- A column dropped or retyped on a table that has an unconsumed stream.
- A stream query failing after a dbt contract change on the source.
- A task that alters the source and reads the stream in the same step without ordering the consume first.

## Fix shape

- Consume the stream before the incompatible alter.
- Recreate the stream after a breaking schema change if the old offset cannot be read.

## Field reports

No thread in the catalog communities stated this failure with a stack and a fix that matches the docs. Search [r/dataengineering](https://www.reddit.com/r/dataengineering/) and the tool tags in `sources/catalog.yaml` before treating a comment as a rule.

## Canonical check

- [Streams](https://docs.snowflake.com/en/user-guide/streams-intro) — Streams use the current table schema and may read deleted data. An incompatible schema change between the offset and the advance can fail the query.

Renaming the source object does not stale the stream. Dropping it and creating a new object with the same name does not attach the old stream to the new object.
