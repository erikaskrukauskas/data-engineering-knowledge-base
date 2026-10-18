# Kafka offsets

The consumer commits an offset only after the corresponding records are durable in the landing table. A replay can insert the same record again, and the landing table can absorb that.

## Look for

- `enable.auto.commit=true` (the client default) on a consumer that writes to a database after `poll`. The offset can advance when the records are returned, before the insert commits.
- A consumer that does not finish the records from one `poll` before the next `poll` or before close, while auto-commit is on. The committed offset can move past records that were never written.
- `auto.offset.reset=latest` on a new consumer group. Messages produced before the group started are skipped.
- A sink that inserts into a normalized table with no key, so a replay creates a second financial transaction.
- A claim that the pipeline is exactly-once because it uses Kafka, with no transactional producer and no dedupe key in the landing table.

## Fix shape

- Set `enable.auto.commit` to false. Call `commitSync` for the offsets of a batch only after that batch is inserted.
- Land records in an append-only table keyed by the event id. Dedupe at read time. Design the sink for at-least-once delivery.
- Set `auto.offset.reset` to `earliest` or `none` when a new group must not silently skip history. `none` fails the consumer when no offset is stored.

## Field reports

- [Kafka events landing out of order, missing, and duplicated](https://www.reddit.com/r/dataengineering/comments/1atccw5/question_on_data_pipeline_and_kafka_events/) — r/dataengineering. Replays duplicated transactions and some events were treated as unrecoverable. The fix that matches the consumer docs is at-least-once delivery into an append-only log, with duplicates removed on the business key.

## Canonical check

- [KafkaConsumer](https://kafka.apache.org/43/javadoc/org/apache/kafka/clients/consumer/KafkaConsumer.html) — auto-commit marks records consumed when `poll` returns them. Manual commit after the database insert is at-least-once: a crash between insert and commit redelivers the batch. Auto-commit is also at-least-once only if every record from a `poll` is handled before the next `poll` or close. Otherwise the committed offset can pass unhandled records.
- [Consumer configs](https://kafka.apache.org/36/configuration/consumer-configs/) — `enable.auto.commit` defaults to true. `auto.offset.reset` defaults to `latest`.

A popular thread treats rebalance as data loss and "exactly-once" as marketing. The consumer javadoc does not say a rebalance drops records. It says a custom consumer that commits after the insert must tolerate duplicates. Exactly-once applies to a Kafka-to-Kafka transactional flow, not to an ordinary database sink.
