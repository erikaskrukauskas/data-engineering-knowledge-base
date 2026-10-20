# Change feed retention

Change data files follow the table retention. VACUUM deletes them with the other files outside that window.

## Look for

- A CDF reader with a starting version older than the vacuum retention.
- A job that treats `_change_data` as a permanent audit folder.
- `VACUUM` right after enabling CDF on a busy table.

## Fix shape

- Keep retention longer than the oldest change version you read.
- Copy the change feed into an audit table when the history must outlive vacuum.

## Field reports

No thread in the catalog communities stated this failure with a stack and a fix that matches the docs. Search [r/dataengineering](https://www.reddit.com/r/dataengineering/) and the tool tags in `sources/catalog.yaml` before treating a comment as a rule.

## Canonical check

- [Change data feed](https://docs.delta.io/delta-change-data-feed/) — Files in `_change_data` follow table retention and are deleted by VACUUM. Change data is committed in the same transaction as the table write.

Insert-only commits and full partition deletes may not write `_change_data` at all. Delta can compute those changes from the log. An empty folder does not mean the feed is off.
