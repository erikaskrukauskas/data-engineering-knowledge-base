# Surrogate keys

The same business grain produces the same key on every run. A rerun updates that row instead of inserting a second one.

## Look for

- `uuid()`, `generate_uuid()`, or `random()` used as the key of a model that is incremental or is rebuilt.
- `max(surrogate_key) + row_number()` computed independently on each warehouse worker.
- A hash that includes the load timestamp or the ingestion filename, so the same entity gets a new key every run.
- A hash of columns that can be null, concatenated without a stand-in for null. `md5(null || business_key)` is null, and a null key does not match on the next run.
- An `insert_overwrite` model that sets `unique_key` to the hash and expects the hash to deduplicate. That strategy replaces a partition and does not use `unique_key`.

## Fix shape

- Hash the columns that define the grain with `dbt_utils.generate_surrogate_key`. The macro coalesces nulls before hashing.
- For a type 1 row, hash only the business grain. For an SCD2 version, hash the business grain plus the columns that define that version, not the clock time of the load.
- Cast timestamps to a stable string before hashing. Set that column as `unique_key` on `merge` and `delete+insert` models.

## Field reports

- [Surrogate key strategy for fact tables](https://www.reddit.com/r/dataengineering/comments/1h0foff/surrogate_key_strategy_for_fact_tables/) — r/dataengineering. An incremental integer or a fresh UUID is a different value on every run. The fix that matches the docs is to cast the grain to strings and hash it.

## Canonical check

- [Idempotence in dbt](https://docs.getdbt.com/best-practices/idempotence) — a random surrogate key is a different value on every run. Use a deterministic hash such as `dbt_utils.generate_surrogate_key`.
- [Incremental models](https://docs.getdbt.com/docs/build/incremental-models) — nulls in `unique_key` fail to match and duplicate rows. `insert_overwrite` ignores `unique_key`.
- [Hashed surrogate keys](https://docs.getdbt.com/blog/managing-surrogate-keys) — the same inputs always produce the same hash, so a rebuild of non-source tables can recreate the keys. MD5 collisions are negligible until the table is astronomically large. A consumer that requires an integer key needs a warehouse hash that returns an integer, which has a smaller space.

A comment on a related thread hashes the business key together with the load date for every table. That is right for an SCD2 version. It is a new key on every load for a type 1 entity, which breaks the idempotence rule above.
