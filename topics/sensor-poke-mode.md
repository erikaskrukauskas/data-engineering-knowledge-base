# Sensor poke mode

A sensor that checks about once a minute uses `mode="reschedule"`. `poke` is the default and holds a worker slot the whole time.

## Look for

- A sensor with no `mode`, waiting on a file or an external task for most of an hour.
- `mode="poke"` with `poke_interval` of 60 or more.
- Dozens of DAG runs blocked because every slot is a sleeping sensor.

## Fix shape

- Set `mode="reschedule"` when the check interval is about a minute.
- Leave `poke` for a check that really is every second and finishes quickly.

## Field reports

No thread in the catalog communities stated this failure with a stack and a fix that matches the docs. Search [r/dataengineering](https://www.reddit.com/r/dataengineering/) and the tool tags in `sources/catalog.yaml` before treating a comment as a rule.

## Canonical check

- [Sensors](https://airflow.apache.org/docs/apache-airflow/stable/core-concepts/sensors.html) — `poke` occupies a worker slot for the whole runtime. `reschedule` occupies a slot only while checking. A per-second check belongs in poke. A per-minute check belongs in reschedule.

Reschedule frees the worker between checks. It still schedules a task run every interval. A sub-minute interval in reschedule is the next note.
