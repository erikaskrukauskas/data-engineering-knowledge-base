# Watermark cleanup conditions

A watermark cleans aggregation state only in append or update mode, on the same column the aggregation uses, and only if it was declared before the aggregation.

## Look for

- Complete mode plus a watermark, expected to expire windows.
- `withWatermark("time")` and `groupBy("time2")`.
- The watermark call chained after `groupBy`.
- Append mode on that invalid plan.

## Fix shape

- Use append or update.
- Watermark the grouped event-time column.
- Call `withWatermark` first.

## Field reports

No thread in the catalog communities stated this failure with a stack and a fix that matches the docs. Search [r/dataengineering](https://www.reddit.com/r/dataengineering/) and the tool tags in `sources/catalog.yaml` before treating a comment as a rule.

## Canonical check

- [Structured Streaming](https://spark.apache.org/docs/latest/streaming/apis-on-dataframes-and-datasets.html) — Complete mode cannot drop aggregate state. The watermark column and the aggregation column must match. The watermark must be applied before the aggregation. The page marks the mismatched examples invalid in append mode.

Meeting the three conditions allows cleanup. It does not emit a late event that arrived after the watermark passed the window.
