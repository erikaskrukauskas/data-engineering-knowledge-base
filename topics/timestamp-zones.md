# Timestamp zones

A timestamp is stored with a zone, and the date used for partitioning is taken in one declared zone.

## Look for

- `current_timestamp()` or `now()` written into a warehouse column that later becomes a partition or a key.
- A naive timestamp parsed as local time in the loader and as UTC in the warehouse.
- `date_trunc` or `DATE()` applied before the zone is set, so the same instant falls on two dates.
- A dbt incremental filter that compares a zoned column to a string with no zone.

## Fix shape

- Store the source timestamp. Convert to UTC in staging when every later model needs UTC, and name the column so the zone is obvious.
- Derive the business date in that same zone. Do not derive it again downstream in a different zone.

## Field reports

- [dbt insert_overwrite duplicates](https://www.reddit.com/r/dataengineering/comments/1l1lpdz/dbt_incremental_models_with_insert_overwrite/) — r/dataengineering. One comment on the duplicate-partition thread traced the split to timestamps that were not in the same zone before the date was extracted. The docs' fix is a source timestamp, converted once.

## Canonical check

- [Idempotence in dbt](https://docs.getdbt.com/best-practices/idempotence) — `current_timestamp()` changes on every run. Use a timestamp that came from the source.
- [dbt staging models](https://docs.getdbt.com/best-practices/how-we-structure/2-staging) — A cast or truncation that every downstream model needs belongs in staging, once.

Airflow data intervals are timezone-aware. A warehouse session timezone can still shift a `DATE(timestamp)` if the column is naive. Check the session zone before blaming the DAG.
