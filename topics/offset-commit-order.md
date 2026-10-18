# Offset commit order

Handle the records, commit the sink, then commit the offset.

## Look for

- An offset commit at the start of `poll` processing.
- `enable.auto.commit=true` (the default) with a handler that may not finish the batch before the next poll or close.
- A sink insert with no key, so the replay that at-least-once requires creates a second row.

## Fix shape

- Disable auto-commit when the sink is a database.
- Commit the offset after the transaction that wrote the rows.
- Give the destination a key so the replay merges.

## Field reports

No thread in the catalog communities stated this failure with a stack and a fix that matches the docs. Search [r/dataengineering](https://www.reddit.com/r/dataengineering/) and the tool tags in `sources/catalog.yaml` before treating a comment as a rule.

## Canonical check

- [Kafka consumer configs](https://kafka.apache.org/36/configuration/consumer-configs/) — `enable.auto.commit` defaults to true.
- [KafkaConsumer](https://kafka.apache.org/43/javadoc/org/apache/kafka/clients/consumer/KafkaConsumer.html) — Committing before the records are handled can skip them. Committing after can redeliver them.

The kafka-offsets note is the general case. This note is the order of the two commits.
