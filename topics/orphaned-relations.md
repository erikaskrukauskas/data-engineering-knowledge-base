# Orphaned relations

Removing a model from the project is followed by a drop of the relation it used to build, in every custom schema.

## Look for

- Models deleted from git while the warehouse table remains, with a dashboard still pointed at it.
- A drop macro that only looks at `target.schema` and misses `+schema` marts.
- `dbt clean` expected to drop warehouse relations. It cleans the local `target` directory, not the warehouse.

## Fix shape

- List relations the project still builds, including custom schemas, and drop the warehouse objects that are no longer in that list.
- Point exposures at the replacement model before the old relation is dropped.

## Field reports

- [Cleaning custom schemas](https://www.reddit.com/r/dataengineering/comments/10wrlve/dbt_clean_your_warehouse_of_old_and_deprecated/) — r/dataengineering. Deprecated models were left behind because the cleanup did not walk custom schemas. The docs' fix is to include every schema the project actually writes.

## Canonical check

- [Custom schemas](https://docs.getdbt.com/docs/build/custom-schemas) — A custom schema changes the relation name. A cleanup that only scans the target schema will not see it.
- [dbt documentation](https://docs.getdbt.com/docs/build/documentation) — An exposure names the consumer that still depends on a model. Drop the relation after that dependency is moved.

Do not drop a snapshot because the model list no longer contains it. Snapshots are not rebuildable from the current source. The snapshot docs say to keep them in their own schema.
