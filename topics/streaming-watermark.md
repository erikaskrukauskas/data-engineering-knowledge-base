# Streaming watermark

A streaming aggregation that must bound state calls `withWatermark` on the event-time column before the aggregation.

## Look for

- A `groupBy(window(...))` with no `withWatermark`.
- `withWatermark` after the aggregation.
- `withWatermark` on a different column from the one inside the window.
- A batch DataFrame calling `withWatermark` and expecting it to filter late rows. On a batch Dataset it is a no-op.

## Fix shape

- Call `withWatermark(eventTime, delay)` on the streaming DataFrame before `groupBy`.
- Use the same column the window uses.
- Set the delay to the lateness you will still accept.

## Field reports

No thread in the catalog communities stated this failure with a stack and a fix that matches the docs. Search [r/dataengineering](https://www.reddit.com/r/dataengineering/) and the tool tags in `sources/catalog.yaml` before treating a comment as a rule.

## Canonical check

- [Structured Streaming](https://spark.apache.org/docs/latest/streaming/apis-on-dataframes-and-datasets.html) — Watermarking drops state once event time is more than the delay past a window end. The call has to be on the aggregation's timestamp column and before the aggregation. On a non-streaming Dataset the call is ignored.

The delay is a minimum guarantee. Data less than the delay late is kept. Data later than the delay may be dropped, and the engine may still process some of it because the watermark is coordinated across partitions.
