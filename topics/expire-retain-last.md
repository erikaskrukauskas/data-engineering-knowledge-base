# Expire and retain last

`expireSnapshots` deletes history up to a timestamp and can retain the last N ancestors anyway.

## Look for

- An expiry example copied with a one-day timestamp onto a table analysts time-travel for a month.
- No expiry at all, so metadata grows with every commit.
- Code that deletes the current snapshot. The API will not.

## Fix shape

- Pass a timestamp that matches the oldest time travel you still offer.
- Set `retainLast` when the newest N snapshots must survive even if they are older than that timestamp.

## Field reports

No thread in the catalog communities stated this failure with a stack and a fix that matches the docs. Search [r/dataengineering](https://www.reddit.com/r/dataengineering/) and the tool tags in `sources/catalog.yaml` before treating a comment as a rule.

## Canonical check

- [ExpireSnapshots](https://iceberg.apache.org/javadoc/latest/org/apache/iceberg/ExpireSnapshots.html) — The call removes snapshots older than the timestamp. `retainLast` keeps the most recent ancestors. The current snapshot is not deleted.
- [Iceberg maintenance](https://iceberg.apache.org/docs/latest/maintenance/) — Regular expiry is what keeps metadata small and lets unreferenced files go.

Data files are removed only when no remaining snapshot references them. Retaining a snapshot retains its files.
