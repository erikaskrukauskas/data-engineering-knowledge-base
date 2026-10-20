# Sensor reschedule interval

In `reschedule` mode the poke interval is more than one minute so the scheduler is not loaded with constant checks.

## Look for

- `mode="reschedule"` and `poke_interval=5`.
- `exponential_backoff` left off while the external system is down, so the sensor returns at a fixed short interval.
- `max_wait` unset while backoff is on, so the delay can grow without the cap you intended.

## Fix shape

- Set `poke_interval` above 60 seconds for reschedule.
- Turn on `exponential_backoff` and set `max_wait` when the remote system is often unavailable.

## Field reports

No thread in the catalog communities stated this failure with a stack and a fix that matches the docs. Search [r/dataengineering](https://www.reddit.com/r/dataengineering/) and the tool tags in `sources/catalog.yaml` before treating a comment as a rule.

## Canonical check

- [Sensors](https://airflow.apache.org/docs/apache-airflow/stable/core-concepts/sensors.html) — Reschedule sleeps between checks. The sensor base class says the poke interval should be more than one minute in that mode, or the scheduler takes the load.
- [Deferrable operators](https://airflow.apache.org/docs/apache-airflow/stable/authoring-and-scheduling/deferring.html) — Reschedule still runs on a time interval. A deferrable operator waits on the triggerer instead.

The one-minute guidance is from the sensor base class. A sensor that must notice a file within seconds should stay in poke, or use a deferrable sensor, rather than a one-second reschedule.
