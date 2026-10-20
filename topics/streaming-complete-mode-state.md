# Complete mode state

Complete output mode is not a way to expire windows. It retains the whole aggregation.

## Look for

- `outputMode("complete")` plus `withWatermark`, with the watermark expected to drop state.
- A sink that receives the full aggregation on every trigger and is sized for the latest window only.
- Complete mode on a join or on a non-aggregated stream.

## Fix shape

- Use append or update when state must expire.
- Use complete only when the sink must receive every group so far, and size it for that.

## Field reports

No thread in the catalog communities stated this failure with a stack and a fix that matches the docs. Search [r/dataengineering](https://www.reddit.com/r/dataengineering/) and the tool tags in `sources/catalog.yaml` before treating a comment as a rule.

## Canonical check

- [Structured Streaming](https://spark.apache.org/docs/latest/streaming/apis-on-dataframes-and-datasets.html) — Complete mode preserves all aggregate data, so watermarking cannot drop it. Complete is unsupported for joins and for queries that are not aggregations.

Update mode does use the watermark to drop old state, and it emits rows that changed. The sink has to accept corrections.
