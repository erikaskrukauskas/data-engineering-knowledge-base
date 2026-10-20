# Stream-stream joins

A stream-stream outer join or semi join sets a watermark on both inputs and a time constraint. An inner join can run without them, and then state is not bounded.

## Look for

- A left join of two streams with no `withWatermark` and no event-time predicate.
- Watermarks on only one side.
- An outer result expected in the same trigger as the left row. The engine waits until the watermark says no match can still arrive.

## Fix shape

- Watermark both inputs.
- Add the event-time range to the join condition.
- Expect the outer null row to appear only after that delay.

## Field reports

No thread in the catalog communities stated this failure with a stack and a fix that matches the docs. Search [r/dataengineering](https://www.reddit.com/r/dataengineering/) and the tool tags in `sources/catalog.yaml` before treating a comment as a rule.

## Canonical check

- [Structured Streaming](https://spark.apache.org/docs/latest/streaming/apis-on-dataframes-and-datasets.html) — Outer and semi joins require watermark delays and a time constraint so the engine knows a row will never match. Inner joins can omit them. Outer nulls are delayed by that watermark, and they wait for the next micro-batch that has data.

If either input goes quiet, outer results wait, because the watermark advances at the end of a micro-batch and a batch runs when new data arrives.
