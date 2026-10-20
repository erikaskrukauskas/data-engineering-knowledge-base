# Stream offset advance

Selecting from a stream does not move its offset. A committed DML statement that reads the stream does.

## Look for

- A task that `SELECT`s the stream into a log and expects the next run to see only new rows.
- Two statements that read the stream without `BEGIN` and `COMMIT`, so the second sees a different delta.
- `CREATE TABLE AS SELECT` forgotten as a consuming statement. It is one.

## Fix shape

- Consume the stream in the INSERT, MERGE, CTAS, or COPY INTO that applies the change.
- Wrap several statements that must see the same delta in one explicit transaction.

## Field reports

No thread in the catalog communities stated this failure with a stack and a fix that matches the docs. Search [r/dataengineering](https://www.reddit.com/r/dataengineering/) and the tool tags in `sources/catalog.yaml` before treating a comment as a rule.

## Canonical check

- [Streams](https://docs.snowflake.com/en/user-guide/streams-intro) — The offset advances only when the stream is used in a DML transaction, including CTAS and COPY INTO. A query alone does not advance it, even inside an explicit transaction, unless the DML consumes the stream.

If the transaction rolls back, the offset does not move. A failed merge can be retried against the same changes.
