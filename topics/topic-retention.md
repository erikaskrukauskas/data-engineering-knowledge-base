# Topic retention

A topic keeps records long enough for the longest replay you promise.

## Look for

- A default retention on a topic that a nightly job rebuilds from the start of the week.
- A compacted topic used as a full history. Compaction keeps the latest key, not every event.
- A downstream table with no raw copy and a topic that expired.

## Fix shape

- Set retention to the replay window.
- Copy events to the raw table before they age out.
- Use compaction only when the latest value per key is the state you want.

## Field reports

No thread in the catalog communities stated this failure with a stack and a fix that matches the docs. Search [r/dataengineering](https://www.reddit.com/r/dataengineering/) and the tool tags in `sources/catalog.yaml` before treating a comment as a rule.

## Canonical check

- [Kafka consumer configs](https://kafka.apache.org/36/configuration/consumer-configs/) — A consumer can only reset to records the broker still has. `earliest` is the earliest retained offset, not the start of time.

This page states the consumer side. The broker retention setting lives on the topic. Check that value in the cluster before promising a replay window.
