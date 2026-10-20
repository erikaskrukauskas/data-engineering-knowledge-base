# RocksDB state memory

`boundedMemoryUsage` on the RocksDB state store is a soft cap. The process can pass it while higher-level readers hold blocks.

## Look for

- A hard memory limit assumed from that config.
- A query killed and retried because the soft limit was treated as a failure.
- No RocksDB provider on a stateful query that no longer fits the JVM heap.

## Fix shape

- Use RocksDB when JVM state is the problem.
- Size the executor for the soft cap plus the temporary overage.
- Do not expect the setting to rebalance state onto more nodes.

## Field reports

No thread in the catalog communities stated this failure with a stack and a fix that matches the docs. Search [r/dataengineering](https://www.reddit.com/r/dataengineering/) and the tool tags in `sources/catalog.yaml` before treating a comment as a rule.

## Canonical check

- [Structured Streaming](https://spark.apache.org/docs/latest/streaming/apis-on-dataframes-and-datasets.html) — The bounded memory config is a soft limit. A strict limit is not available because it would fail the query, and rebalancing state across nodes is not supported.

Changelog checkpointing is a separate flag and defaults off. Turning it on does not make the memory cap strict.
