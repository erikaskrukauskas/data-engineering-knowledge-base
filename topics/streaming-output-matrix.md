# Streaming output modes

Aggregations without a watermark support complete and update, not append. Joins support append. Unaggregated queries do not support complete.

## Look for

- `outputMode("append")` on a count with no watermark.
- `outputMode("complete")` on a query that does not aggregate.
- `outputMode("update")` on a stream-stream join.
- `mapGroupsWithState` written in append mode. That operator supports update.

## Fix shape

- Pick the mode from the query type.
- Add a watermark before using append on a windowed aggregation.
- Use update for `mapGroupsWithState`.

## Field reports

No thread in the catalog communities stated this failure with a stack and a fix that matches the docs. Search [r/dataengineering](https://www.reddit.com/r/dataengineering/) and the tool tags in `sources/catalog.yaml` before treating a comment as a rule.

## Canonical check

- [Structured Streaming](https://spark.apache.org/docs/latest/streaming/apis-on-dataframes-and-datasets.html) — The compatibility table says aggregations without a watermark are complete or update. Joins are append. Complete is not supported for queries that would have to retain every unaggregated row. `mapGroupsWithState` is update.

`flatMapGroupsWithState` follows its own operation mode: append or update. Aggregations after it are allowed only in the append operation mode.
