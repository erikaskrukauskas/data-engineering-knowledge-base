# Late data bound

The watermark delay is the bound you promise. Events older than that bound, relative to the max event time, can be dropped.

## Look for

- `withWatermark(..., "1 minute")` on a source that arrives hours late.
- A watermark of days on a high-cardinality window, so state is kept for days.
- Processing time used as the watermark column. The column has to be event time.

## Fix shape

- Set the delay to the real lateness.
- Shorten it only when dropping later events is acceptable and state size requires it.

## Field reports

No thread in the catalog communities stated this failure with a stack and a fix that matches the docs. Search [r/dataengineering](https://www.reddit.com/r/dataengineering/) and the tool tags in `sources/catalog.yaml` before treating a comment as a rule.

## Canonical check

- [Structured Streaming](https://spark.apache.org/docs/latest/streaming/apis-on-dataframes-and-datasets.html) — A delay of two hours guarantees events less than two hours late are aggregated. Later events may be dropped. The watermark is max event time minus the delay, and coordination can leave it at least that far behind.

Append mode then waits out that delay before the window is final. A long delay is both a completeness promise and an output lag.
