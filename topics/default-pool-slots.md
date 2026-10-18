# Default pool slots

Slot count for `default_pool` is changed on the running deployment. The config default applies when the pool is created.

## Look for

- `AIRFLOW__CORE__DEFAULT_POOL_TASK_SLOT_COUNT` raised on a cluster where `default_pool` already exists, with no change in the UI.
- Every warehouse task left in `default_pool`, so a backfill takes all 128 slots.
- A task with `pool_slots` greater than 1 and no pool of its own.

## Fix shape

- Change slots in the UI, API, or CLI on an existing deployment.
- Put warehouse-heavy tasks in a pool whose slot count matches the warehouse concurrency.

## Field reports

No thread in the catalog communities stated this failure with a stack and a fix that matches the docs. Search [r/dataengineering](https://www.reddit.com/r/dataengineering/) and the tool tags in `sources/catalog.yaml` before treating a comment as a rule.

## Canonical check

- [Configuration reference](https://airflow.apache.org/docs/apache-airflow/stable/configurations-ref.html) — The default slot count is 128. The setting has no effect once `default_pool` has been created.

Pools limit Airflow task slots. They do not limit Snowflake warehouse concurrency unless the pool size was chosen to match it.
