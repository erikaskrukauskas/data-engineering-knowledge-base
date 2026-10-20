# Sensor soft fail

`soft_fail` is false unless a missed file or event should skip downstream work instead of paging.

## Look for

- `soft_fail=True` on the only sensor that means the day's data arrived.
- A skipped sensor treated as success by a trigger rule that runs downstream on skipped.
- `never_fail` and `soft_fail` both set. They are mutually exclusive.

## Fix shape

- Leave `soft_fail` false when a person should hear about the miss.
- Set it true only when the downstream task is optional and a skip is the documented outcome.

## Field reports

No thread in the catalog communities stated this failure with a stack and a fix that matches the docs. Search [r/dataengineering](https://www.reddit.com/r/dataengineering/) and the tool tags in `sources/catalog.yaml` before treating a comment as a rule.

## Canonical check

- [Sensors](https://airflow.apache.org/docs/apache-airflow/stable/core-concepts/sensors.html) — `soft_fail=True` marks the sensor skipped instead of failed when the timeout is reached.

A skip is not a success. Check the downstream trigger rule before assuming skipped means the rest of the DAG still represents a complete load.
