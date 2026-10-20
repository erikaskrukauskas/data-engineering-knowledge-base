# Watermark output mode

Append mode emits a window only after the watermark passes it. Update mode can emit partial counts. Complete mode never drops the aggregate state.

## Look for

- Append mode chosen for a file sink, with no watermark, on an aggregation. Append is unsupported for aggregations that have no watermark.
- Complete mode used to bound state. It keeps every aggregate.
- A file sink in update mode. File sinks need the finalized rows append mode writes.

## Fix shape

- Use append with a watermark when the sink wants one final row per window.
- Use update when partial counts must be written and the sink supports updates.
- Do not use complete mode as a retention policy.

## Field reports

No thread in the catalog communities stated this failure with a stack and a fix that matches the docs. Search [r/dataengineering](https://www.reddit.com/r/dataengineering/) and the tool tags in `sources/catalog.yaml` before treating a comment as a rule.

## Canonical check

- [Structured Streaming](https://spark.apache.org/docs/latest/streaming/apis-on-dataframes-and-datasets.html) — Append and update can drop aggregation state with a watermark. Complete mode keeps it. Append delays output until the window is finalized. Aggregations without a watermark do not support append.

Joins support append mode. Update and complete are not supported for joins. A join query cannot be switched to update to make late rows appear sooner.
