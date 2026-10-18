# Incremental lookback

An incremental filter that ignores late rows will keep ignoring them on the next run.

## Look for

- A filter on `loaded_at >= current_date` with no lookback.
- `lag_tolerance` set and the source `loaded_at_query` covering a shorter window.
- A partition on event date and a filter on extract time, so a late event lands in a partition the next run does not touch.

## Fix shape

- Look back by the lateness you accept.
- Make `loaded_at_query` and the incremental predicate the same window.
- Partition on the column you filter.

## Field reports

No thread in the catalog communities stated this failure with a stack and a fix that matches the docs. Search [r/dataengineering](https://www.reddit.com/r/dataengineering/) and the tool tags in `sources/catalog.yaml` before treating a comment as a rule.

## Canonical check

- [Incremental models](https://docs.getdbt.com/docs/build/incremental-models) — Rows that fall outside the incremental filter are not updated. A late row needs a window that still includes it.
- [Freshness config](https://docs.getdbt.com/reference/resource-configs/freshness) — `loaded_at_query` is the custom clock. It has to agree with the model that reads the source.

Widening the lookback makes the merge larger. That is the cost of not dropping late rows. It is not a reason to use `now()` as the window.
