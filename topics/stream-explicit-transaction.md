# Stream transaction lock

Several statements that read the same stream delta run inside `BEGIN` and `COMMIT`. The commit is what advances the offset.

## Look for

- Two autocommit statements against one stream, the second reading an empty delta.
- A task that inserts from the stream and then updates an audit table from the stream again.
- Parallel DML on the source expected to appear inside the open transaction. It is tracked, and it shows up after the transaction commits.

## Fix shape

- Open one transaction for every statement that needs that delta.
- Commit once.
- Let later source changes land in the next consume.

## Field reports

No thread in the catalog communities stated this failure with a stack and a fix that matches the docs. Search [r/dataengineering](https://www.reddit.com/r/dataengineering/) and the tool tags in `sources/catalog.yaml` before treating a comment as a rule.

## Canonical check

- [Streams](https://docs.snowflake.com/en/user-guide/streams-intro) — A committed DML consume advances the offset. Multiple statements need an explicit transaction to see the same records. Source DML in other transactions is tracked but does not change the open stream until commit.

A query that does not consume the stream does not advance the offset even if it sits inside the explicit transaction.
