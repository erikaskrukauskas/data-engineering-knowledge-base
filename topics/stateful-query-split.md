# Stateful query split

Multiple stateful operations in one streaming query are split so each query has one, when the combined plan cannot give end-to-end exactly-once.

## Look for

- A watermarked aggregation and a second stateful step in one `writeStream`.
- A recovery that duplicates or drops rows across two state stores in one query.
- RocksDB enabled on a query whose JVM state was small, with changelog checkpointing assumed on. It is off by default.

## Fix shape

- Split into one stateful operation per query.
- Turn on RocksDB when state is larger than the executor heap.
- Set `spark.sql.streaming.stateStore.rocksdb.changelogCheckpointing.enabled` only after you want changelog checkpoints. The default is off, and it can be turned off again without discarding the old checkpoint.

## Field reports

No thread in the catalog communities stated this failure with a stack and a fix that matches the docs. Search [r/dataengineering](https://www.reddit.com/r/dataengineering/) and the tool tags in `sources/catalog.yaml` before treating a comment as a rule.

## Canonical check

- [Structured Streaming](https://spark.apache.org/docs/latest/streaming/apis-on-dataframes-and-datasets.html) — A known workaround for some multi-state plans is one stateful operation per query. RocksDB keeps state off-heap and still checkpoints it. Changelog checkpointing is off by default and is compatible with the older snapshot checkpoints.

Preferred location for state is not a hard scheduler guarantee. A task can still land on another executor and reload the checkpoint. Locality settings reduce that, they do not forbid it.
