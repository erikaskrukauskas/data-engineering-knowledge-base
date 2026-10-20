# State store locality

Stateful streaming tasks prefer the executor that already holds the state store, because loading a checkpoint onto a new executor adds a batch of latency.

## Look for

- A cluster that reschedules every micro-batch onto new executors.
- `loadedMapCacheMissCount` climbing on the HDFS state store.
- Locality waits set to zero on a query with large state.

## Fix shape

- Watch `loadedMapCacheHitCount` and `loadedMapCacheMissCount`.
- Raise Spark locality waiting so the state store is reused.
- Expect a miss to reload from the checkpoint. Preferred location is not mandatory.

## Field reports

No thread in the catalog communities stated this failure with a stack and a fix that matches the docs. Search [r/dataengineering](https://www.reddit.com/r/dataengineering/) and the tool tags in `sources/catalog.yaml` before treating a comment as a rule.

## Canonical check

- [Structured Streaming](https://spark.apache.org/docs/latest/streaming/apis-on-dataframes-and-datasets.html) — Reloading state from the checkpoint hurts micro-batch latency. The HDFS state store exposes hit and miss counts. Locality settings reduce misses. The scheduler can still place the task elsewhere.

RocksDB has the same checkpoint reload cost when the provider moves. Changelog checkpointing changes what is uploaded. It does not pin the executor.
