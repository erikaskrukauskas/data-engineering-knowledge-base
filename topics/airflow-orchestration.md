# Airflow orchestration

Airflow decides when work runs and whether it succeeded. The warehouse, Spark, or another worker holds the data.

## Look for

- Pandas or other in-process transforms on the worker for data that does not fit in worker memory.
- XCom values that carry rows, files, or dataframes instead of a path or a key.
- `Variable.get`, connection lookups, or other network calls at DAG import time. The scheduler parses the file repeatedly.
- A heavy import such as `pandas` at module level in a DAG file. Parsing runs that import on every scheduler pass.
- Files downloaded to the worker with no guaranteed cleanup.
- Schedules of a few minutes on a DAG that watches a folder, with no external trigger.
- A single DAG of dozens of tasks that different teams must backfill on different dates.
- Incremental logic that asks the target for `max(load_date)` and then loads "everything after that," so a rerun and a backfill see different windows.
- `schedule_interval` or `airflow.datasets.Dataset` on Airflow 3. Both were removed in 3.0. The schedule parameter is `schedule`.

## Fix shape

- Pass `data_interval_start` and `data_interval_end` into the task. A rerun of that interval does the same read and write.
- On Airflow 3, set `schedule` instead of `schedule_interval`.
- Push a storage path or a primary key through XCom. Write the payload to object storage or the warehouse.
- Move import-time lookups and heavy imports inside the task callable.
- Delete local files in a `finally` block or use a temporary directory that cleans up on exit.
- Prefer an external event over a sensor that polls every few minutes.
- Split DAGs on schedule and ownership. Use a sensor or a dataset when a downstream DAG only needs to know that an upstream run finished.
- Alert on task failure. Watch the metadata database size.

Schedules tighter than about every 10 minutes need a stated reason. On a busy scheduler, parse and queue time can exceed the interval.

## Field reports

- [Airflow DAG tips](https://www.reddit.com/r/dataengineering/comments/1lbenix/any_airflow_orchestrating_dags_tips/) — r/dataengineering. Commenters describe out-of-memory workers, lost logs, large XComs, and import-time database calls.
- Tag page: [`airflow` sorted by votes](https://stackoverflow.com/questions/tagged/airflow?tab=Votes)

## Canonical check

[Airflow best practices](https://airflow.apache.org/docs/apache-airflow/stable/best-practices.html) (3.3.2) — idempotent tasks, explicit partitions, `default_args` for repeated connection and path settings, small XCom payloads, and no expensive work while the scheduler parses the file.

On Airflow 3, replace `schedule_interval` with `schedule`. The 3.0 page removes the `airflow.datasets.Dataset` import and does not show the replacement in the best-practices example, so confirm the current import before editing a dataset-triggered DAG. Tests that call `TaskInstance.run()` or `TaskInstance.render_templates()` fail on 3.2; call `op.execute(context)` and `op.render_template_fields(context)`. Check the major version in the project before applying a comment from the thread.
