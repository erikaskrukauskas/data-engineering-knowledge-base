# Ratio filter scope

A filter on the ratio applies to both numerator and denominator. A filter on one side is set on that side's metric dict.

## Look for

- A ratio `filter` meant to limit only the numerator.
- Numerator and denominator filters copied onto the ratio as well, so the predicate runs twice.
- A ratio whose numerator and denominator come from different semantic models but the metric is nested on one model.

## Fix shape

- Put a shared predicate on the ratio `filter`.
- Put a one-sided predicate inside the numerator or denominator dict.
- Move a cross-model ratio to the top-level `metrics` list.

## Field reports

No thread in the catalog communities stated this failure with a stack and a fix that matches the docs. Search [r/dataengineering](https://www.reddit.com/r/dataengineering/) and the tool tags in `sources/catalog.yaml` before treating a comment as a rule.

## Canonical check

- [Creating metrics](https://docs.getdbt.com/docs/build/metrics-overview) — A ratio filter applies to both sides. Each side can also carry its own filter. Cross-model ratios are top-level metrics.

`numerator` and `denominator` must name metrics. They are not column expressions.
