# View change tracking

Change tracking is enabled on the view and on every underlying table before a stream on that view returns changes.

## Look for

- `CREATE STREAM` on a view while the table owner never set `CHANGE_TRACKING`.
- A secure view with no `CHANGE_TRACKING = TRUE`.
- The view owner and the table owner are different roles, and only the view was altered.

## Fix shape

- Have the view owner enable change tracking on the view.
- Have each table owner enable it on the underlying tables.
- Creating the stream as the owner of both turns tracking on for both.

## Field reports

No thread in the catalog communities stated this failure with a stack and a fix that matches the docs. Search [r/dataengineering](https://www.reddit.com/r/dataengineering/) and the tool tags in `sources/catalog.yaml` before treating a comment as a rule.

## Canonical check

- [Manage streams](https://docs.snowflake.com/en/user-guide/streams-manage) — Only the owner can enable change tracking. A stream created by the view owner enables it on the view, and on the tables only when that role owns them too.
- [Streams](https://docs.snowflake.com/en/user-guide/streams-intro) — View streams need change tracking on the view and the underlying tables.

Enabling change tracking locks the object for the statement. Expect DDL and DML latency while it runs. Replacing an underlying table stales the view's stream.
