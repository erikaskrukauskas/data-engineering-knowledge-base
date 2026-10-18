# Streaming completeness

A streaming aggregate declares the window and the watermark that mean the window is closed. It does not wait for a last record that will never arrive.

## Look for

- A job that blocks until a Kafka topic is empty.
- An aggregate emitted with no watermark and no window end.
- A retention setting shorter than the replay the downstream job needs.

## Fix shape

- Define the window and the lateness you will accept. Emit when the watermark passes the window end.
- Keep topic retention at least as long as the replay you promise.
- Land the raw events so a late record can be applied without pretending the stream ended.

## Field reports

- [How do you know a stream is complete?](https://www.reddit.com/r/dataengineering/comments/1k41dxp/streaming_how_do_you_know_if_things_are_complete/) — r/dataengineering. The question treated a stream like a file that finishes. The replies that match the docs separate an unbounded stream from a bounded batch and point at offsets plus retention for replay.

## Canonical check

- [Kafka consumer configs](https://kafka.apache.org/36/configuration/consumer-configs/) — `auto.offset.reset` and retention decide what a new or late consumer can still read. `latest` skips history. Expired records are gone.
- [Iceberg maintenance](https://iceberg.apache.org/docs/latest/maintenance/) — Snapshots are the replay point for a table. Expire them only after the consumers of that history are done.

This note does not pick a window type. Tumbling, sliding, and session windows close differently. The failure is a pipeline with no definition of closed.
