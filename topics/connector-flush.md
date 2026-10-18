# Connector flush

A Kafka Connect sink flushes on size or on time, and the offset advances only after that flush lands.

## Look for

- `flush.size` larger than the topic ever holds, so the connector rereads the same offset and rewrites the same object.
- No `rotate.interval.ms` beside a large `flush.size`.
- A sink that overwrites one object per partition instead of writing a new object for each committed batch.

## Fix shape

- Set a time-based rotate interval next to the size threshold so a quiet topic still flushes.
- Confirm the topic has more records than `flush.size` before raising that setting.
- Make the object name include the committed offset range so a replay does not silently overwrite a different batch.

## Field reports

- [S3 sink rewrites the same object](https://www.reddit.com/r/dataengineering/comments/wfvaub/kafka_s3_sink_connector_does_not_dump_data/) — r/dataengineering. `flush.size=1000` never filled, so the connector kept the same offset and overwrote the same S3 object. A time rotate was the fix that lets a small topic flush.

## Canonical check

- [KafkaConsumer](https://kafka.apache.org/43/javadoc/org/apache/kafka/clients/consumer/KafkaConsumer.html) — An offset that advances before the write is durable drops records. A commit after the write is at-least-once and can duplicate. The sink has to tolerate the duplicate.

Connect configuration names vary by connector. The failure mode is the same: the offset does not move until the flush condition is met, and a too-large flush never meets it.
