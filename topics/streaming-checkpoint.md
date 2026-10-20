# Streaming checkpoint

A streaming query that must restart sets `checkpointLocation` to a path on a fault-tolerant file system, used by that query.

## Look for

- `writeStream.start()` with no `checkpointLocation` on a file or Kafka sink.
- A checkpoint on local disk of one worker.
- A memory sink treated as recoverable. The memory sink is for debugging. In complete mode a restart rebuilds the table. It is not a durable checkpoint.

## Fix shape

- Set `checkpointLocation` on the query.
- Put it on durable storage.
- Use a sink that can replay from that checkpoint.

## Field reports

No thread in the catalog communities stated this failure with a stack and a fix that matches the docs. Search [r/dataengineering](https://www.reddit.com/r/dataengineering/) and the tool tags in `sources/catalog.yaml` before treating a comment as a rule.

## Canonical check

- [Structured Streaming](https://spark.apache.org/docs/latest/streaming/apis-on-dataframes-and-datasets.html) — Checkpoint location is where fault-tolerant sinks store recovery information, and it should be a directory in a fault-tolerant file system. Some sources cannot replay from checkpointed offsets. The memory sink is not fault tolerant.

Some sources are not recoverable even with a checkpoint. The page says to read the source's fault-tolerance note before promising exactly-once from that source.
