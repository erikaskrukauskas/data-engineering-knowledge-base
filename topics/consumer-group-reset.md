# Consumer group reset

`auto.offset.reset` defaults to `latest`. A new group skips history unless you set `earliest` or a real offset.

## Look for

- A renamed `group.id` that was expected to replay the topic.
- `latest` in production for a job that must rebuild a table from the start of the topic.
- No retained raw table, so the skipped history cannot be recovered once retention passes.

## Fix shape

- Keep a stable group id for a pipeline that should resume.
- Set `earliest` only for a new group that is supposed to read history that is still inside retention.
- Land raw events before they expire.

## Field reports

No thread in the catalog communities stated this failure with a stack and a fix that matches the docs. Search [r/dataengineering](https://www.reddit.com/r/dataengineering/) and the tool tags in `sources/catalog.yaml` before treating a comment as a rule.

## Canonical check

- [Kafka consumer configs](https://kafka.apache.org/36/configuration/consumer-configs/) — `auto.offset.reset` defaults to `latest`. It applies when there is no committed offset for the group.

`earliest` does not recover records the broker has already deleted. Retention is the other limit.
