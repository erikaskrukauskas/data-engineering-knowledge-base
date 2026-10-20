# Change feed writer version

After `delta.enableChangeDataFeed` is set, writers must be newer than Delta Lake 1.2.1. Older writers can still read.

## Look for

- A job pinned to Delta 1.2.1 that appends to a CDF table.
- A mixed cluster where one pipeline enables CDF and another still uses the old writer.
- A read failure blamed on the version. The page says old versions can read.

## Fix shape

- Upgrade the writer past 1.2.1 before enabling the feed.
- Keep old readers if they only read.

## Field reports

No thread in the catalog communities stated this failure with a stack and a fix that matches the docs. Search [r/dataengineering](https://www.reddit.com/r/dataengineering/) and the tool tags in `sources/catalog.yaml` before treating a comment as a rule.

## Canonical check

- [Change data feed](https://docs.delta.io/delta-change-data-feed/) — Enabling the feed means the table can no longer be written with Delta Lake 1.2.1 or below. Reads from those versions still work.

This is a writer protocol change. It is separate from the column-mapping limitations, which depend on the 2.x and 3.x behavior.
