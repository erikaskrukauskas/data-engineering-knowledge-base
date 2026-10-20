# Deferrable sensors

A sensor that waits on an external event uses deferrable execution when that sensor supports it. Reschedule is the fallback that still re-runs the task on a timer.

## Look for

- A cluster full of idle poke sensors.
- `mode="reschedule"` on a sensor that has `deferrable=True` available and a triggerer running.
- Deferrable tasks submitted with no triggerer process, so the deferred task never resumes.

## Fix shape

- Prefer the deferrable form when the provider implements it.
- Run a triggerer. Deferral moves the wait off the worker and onto that process.
- Use reschedule when the sensor has no deferrable mode.

## Field reports

No thread in the catalog communities stated this failure with a stack and a fix that matches the docs. Search [r/dataengineering](https://www.reddit.com/r/dataengineering/) and the tool tags in `sources/catalog.yaml` before treating a comment as a rule.

## Canonical check

- [Deferrable operators](https://airflow.apache.org/docs/apache-airflow/stable/authoring-and-scheduling/deferring.html) — A deferred operator frees the worker and waits in the triggerer. Reschedule only wakes on a time interval and still costs repeated task runs.

`deferrable=True` is a convention implemented by some operators. It is not a mode on every sensor. Check the sensor you use before adding the flag.
