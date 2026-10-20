# Delta change feed enablement

`delta.enableChangeDataFeed` is set on the table before the first change a downstream job must see. Past changes are not backfilled.

## Look for

- `readChangeFeed` or `table_changes` on a table that never set the property.
- The property altered on Friday with a reader asking for Monday's version.
- A session default `spark.databricks.delta.properties.defaults.enableChangeDataFeed` assumed to alter tables that already exist.

## Fix shape

- Set the table property at `CREATE TABLE`, or `ALTER TABLE` before the changes you need.
- Use the session default only for tables created after it is set.
- Start readers at the version where the feed was enabled.

## Field reports

No thread in the catalog communities stated this failure with a stack and a fix that matches the docs. Search [r/dataengineering](https://www.reddit.com/r/dataengineering/) and the tool tags in `sources/catalog.yaml` before treating a comment as a rule.

## Canonical check

- [Change data feed](https://docs.delta.io/delta-change-data-feed/) — The feed is off by default. Only changes after enablement are recorded. A read that starts before that version errors because the feed was not enabled.

Enabling the feed blocks writes from Delta Lake 1.2.1 and older. Readers on those versions can still read the table.
