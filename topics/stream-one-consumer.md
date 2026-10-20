# One stream per consumer

Each consumer has its own stream. The first committed DML consume advances the offset and hides those changes from the next reader.

## Look for

- One stream named in two tasks.
- A second job that finds the stream empty after the first job's merge.
- A belief that querying is enough for the second consumer. It is not, and it would not share the delta after the first consume anyway.

## Fix shape

- Create a stream per task or script.
- Use Time Travel only when you intentionally re-read an old offset.

## Field reports

No thread in the catalog communities stated this failure with a stack and a fix that matches the docs. Search [r/dataengineering](https://www.reddit.com/r/dataengineering/) and the tool tags in `sources/catalog.yaml` before treating a comment as a rule.

## Canonical check

- [Streams](https://docs.snowflake.com/en/user-guide/streams-intro) — The docs recommend a separate stream per consumer. After one DML consume, the next consumer does not see that delta. Extra streams store offsets, not a second copy of the table.

Several statements in one explicit transaction still count as one consumer. They lock the stream and see the same changes until commit.
