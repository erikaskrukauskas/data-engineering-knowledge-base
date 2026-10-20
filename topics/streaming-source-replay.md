# Streaming source replay

Checkpointed offsets help after a failure only when the source can replay them.

## Look for

- A socket or other non-replayable source plus a checkpoint, described as exactly-once.
- A production job on a source the page marks as not fault tolerant.
- A sink checkpoint with no matching source replay.

## Fix shape

- Use a source that can replay from the checkpointed offsets.
- Read that source's fault-tolerance note before promising recovery.

## Field reports

No thread in the catalog communities stated this failure with a stack and a fix that matches the docs. Search [r/dataengineering](https://www.reddit.com/r/dataengineering/) and the tool tags in `sources/catalog.yaml` before treating a comment as a rule.

## Canonical check

- [Structured Streaming](https://spark.apache.org/docs/latest/streaming/apis-on-dataframes-and-datasets.html) — Some sources are not fault tolerant because they cannot replay data from checkpointed offsets after a failure.

A fault-tolerant sink does not repair a source that cannot replay. Both sides have to support the recovery.
