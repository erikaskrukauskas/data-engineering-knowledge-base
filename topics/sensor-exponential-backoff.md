# Sensor backoff

`exponential_backoff` lengthens the wait between sensor checks, and `max_wait` caps it.

## Look for

- A sensor polling an API every few seconds during an outage.
- `exponential_backoff=True` with no `max_wait`, so the gap grows past the time the file was actually needed.
- Backoff enabled in poke mode on a sensor that already holds a worker slot for the whole wait.

## Fix shape

- Enable backoff on a reschedule or deferrable sensor that calls an unstable system.
- Set `max_wait` to the longest gap you can tolerate.
- Keep a short fixed interval only when the condition is expected within seconds.

## Field reports

No thread in the catalog communities stated this failure with a stack and a fix that matches the docs. Search [r/dataengineering](https://www.reddit.com/r/dataengineering/) and the tool tags in `sources/catalog.yaml` before treating a comment as a rule.

## Canonical check

- [Sensors](https://airflow.apache.org/docs/apache-airflow/stable/core-concepts/sensors.html) — `exponential_backoff` increases the delay up to `max_wait`. It is for external systems whose availability is unpredictable.

Backoff does not change `timeout`. The sensor still fails when the total timeout from the first poke is reached.
