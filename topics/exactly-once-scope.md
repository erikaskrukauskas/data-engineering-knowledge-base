# Exactly-once scope

Kafka's exactly-once guarantee is for Kafka-to-Kafka transactions. A consumer that writes to a warehouse is at-least-once unless the sink dedupes.

## Look for

- A consumer described as exactly-once because the cluster has transactions enabled.
- `enable.auto.commit` left at true while the sink inserts rows.
- A unique key missing on the destination, so a replay duplicates.
- A claim that a rebalance deleted events. A rebalance does not by itself drop records.

## Fix shape

- Commit the offset after the warehouse write succeeds.
- Merge on a key the event already has.
- Use a Kafka transaction only for a consume-transform-produce flow that stays inside Kafka.

## Field reports

No thread in the catalog communities stated this failure with a stack and a fix that matches the docs. Search [r/dataengineering](https://www.reddit.com/r/dataengineering/) and the tool tags in `sources/catalog.yaml` before treating a comment as a rule.

## Canonical check

- [KafkaConsumer](https://kafka.apache.org/43/javadoc/org/apache/kafka/clients/consumer/KafkaConsumer.html) — The consumer documents at-least-once when you commit after handling, and the loss window when you commit before. Exactly-once in the client is the transactional producer path, not a side effect of any sink.

A thread that calls exactly-once a lie is right that a custom database sink is not exactly-once, and wrong if it says a rebalance drops records. The javadoc is the check.
