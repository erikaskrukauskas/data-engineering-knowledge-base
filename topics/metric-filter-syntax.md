# Metric filter syntax

A metric `filter` references entities, dimensions, time, and metrics with the MetricFlow helpers.

## Look for

- `filter: country = 'MX'` with no `Dimension()` call.
- A time filter that names the column without `TimeDimension` and a grain.
- A filter that uses the unqualified dimension name when the qualified form is `entity__dimension`.

## Fix shape

- Use `Entity`, `Dimension`, `TimeDimension`, or `Metric` in the filter string.
- Qualify dimensions as `primary_entity__dimension_name`.

## Field reports

No thread in the catalog communities stated this failure with a stack and a fix that matches the docs. Search [r/dataengineering](https://www.reddit.com/r/dataengineering/) and the tool tags in `sources/catalog.yaml` before treating a comment as a rule.

## Canonical check

- [Creating metrics](https://docs.getdbt.com/docs/build/metrics-overview) — Filters use Jinja helpers: `Entity`, `Dimension('entity__name')`, `TimeDimension('name', 'grain')`, and `Metric`.

The filter is a MetricFlow expression. A warehouse function that worked in the mart SQL is not automatically valid here.
