# Chained window time

A follow-on window uses `window_time` when it needs a timestamp for an existing window column.

## Look for

- A second `window()` call passed the window struct.
- An hour rollup of five-minute windows that groups on the struct fields by hand and splits one window.
- A batch-style `date_trunc` copied into the streaming plan on the window column.

## Fix shape

- Call `window_time` on the window column.
- Pass that timestamp into the next `window`.

## Field reports

No thread in the catalog communities stated this failure with a stack and a fix that matches the docs. Search [r/dataengineering](https://www.reddit.com/r/dataengineering/) and the tool tags in `sources/catalog.yaml` before treating a comment as a rule.

## Canonical check

- [Structured Streaming](https://spark.apache.org/docs/latest/streaming/apis-on-dataframes-and-datasets.html) — `window_time` returns the timestamp that represents the window. That value is what a later window or any timestamp expression should receive.

The page also allows the `window` SQL function on a window column. Use one of those two. Do not treat the struct as an event time.
