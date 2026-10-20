# Sensor timeout clock

`timeout` is the total wait from the first attempt. `execution_timeout` is the running time of one poke and ignores reschedule sleep.

## Look for

- `execution_timeout` of one hour used as the only limit on a reschedule sensor. The running time of each poke is seconds, so the hour never elapses.
- `timeout` shorter than one poke's `execution_timeout`.
- A retry count expected to reset the sensor timeout. It does not.

## Fix shape

- Set `timeout` to the longest the DAG should wait for the condition.
- Set `execution_timeout` to the longest one poke may run.
- Keep `retries` for poke errors other than the sensor timeout.

## Field reports

No thread in the catalog communities stated this failure with a stack and a fix that matches the docs. Search [r/dataengineering](https://www.reddit.com/r/dataengineering/) and the tool tags in `sources/catalog.yaml` before treating a comment as a rule.

## Canonical check

- [Tasks](https://airflow.apache.org/docs/apache-airflow/stable/core-concepts/tasks.html) — In reschedule mode, `timeout` includes the time between pokes and a breach raises `AirflowSensorTimeout` with no retry. `execution_timeout` bounds one poke and can retry. Retries do not reset `timeout`.

In poke mode the two clocks match, because the sensor never reschedules. The split matters when `mode` is reschedule.
