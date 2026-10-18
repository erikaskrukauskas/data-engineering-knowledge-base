# Iceberg snapshot expiry

Snapshots are expired on a schedule so unused files can be deleted, and expiry does not remove the current snapshot.

## Look for

- A table that never calls `expireSnapshots`, so metadata and unreferenced files grow with every write.
- An expiry timestamp that removes snapshots a downstream time-travel query still uses.
- Code that deletes data files itself instead of expiring snapshots and letting unreferenced files go.
- An attempt to expire the current snapshot.

## Fix shape

- Expire snapshots older than the retention readers need, and retain the last N ancestors when recent history must survive.
- Use the Spark expire action for large tables.
- Remember data files stay until no remaining snapshot references them.

## Field reports

No thread in the catalog communities stated this failure with a stack and a fix that matches the docs. Search [r/dataengineering](https://www.reddit.com/r/dataengineering/) and the tool tags in `sources/catalog.yaml` before treating a comment as a rule.

## Canonical check

- [Iceberg maintenance](https://iceberg.apache.org/docs/latest/maintenance/) — Every write creates a snapshot. Expiring snapshots removes them from time travel and allows unused data files to be deleted. Regular expiry keeps metadata small.
- [ExpireSnapshots](https://iceberg.apache.org/javadoc/latest/org/apache/iceberg/ExpireSnapshots.html) — The API will not delete the current snapshot. `retainLast` keeps the most recent ancestors even if they are older than the timestamp.

Expiring a snapshot removes it from time travel. Pick the timestamp from the oldest query you still need to reproduce, not from a default of one day copied out of the example.
