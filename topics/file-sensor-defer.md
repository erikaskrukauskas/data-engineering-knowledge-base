# File sensor mode

Waiting for a file uses `FileSensor`, and a long wait uses its deferrable mode or sensor reschedule rather than a bash loop in poke.

## Look for

- `BashSensor` running `test -f` in the default poke mode for the whole file arrival window.
- A Python loop inside a Python operator that sleeps until a file exists.
- `FileSensor` available and unused.

## Fix shape

- Use `FileSensor` for a filesystem wait.
- Use deferrable mode when that sensor offers it, or `mode="reschedule"` with an interval over a minute.

## Field reports

No thread in the catalog communities stated this failure with a stack and a fix that matches the docs. Search [r/dataengineering](https://www.reddit.com/r/dataengineering/) and the tool tags in `sources/catalog.yaml` before treating a comment as a rule.

## Canonical check

- [Sensors](https://airflow.apache.org/docs/apache-airflow/stable/core-concepts/sensors.html) — The sensor page points file waits at `FileSensor` and notes its deferrable mode. Poke holds a worker for the whole wait.

A file on a worker-local disk is not visible to every worker. The path has to be shared, or the sensor and the writer must be aimed at the same place.
