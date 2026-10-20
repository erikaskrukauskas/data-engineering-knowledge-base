# Derived metric aliases

`expr` on a derived metric refers to the aliases declared on `input_metrics`. `offset_window` is only valid there.

## Look for

- `expr: revenue - cost` while the input metrics are named `order_total` and `order_cost` and have no aliases.
- `offset_window` on a simple or cumulative metric.
- `offset_window` and `offset_to_grain` used as if they were allowed on every metric type.

## Fix shape

- Set `alias` on each input and use those names in `expr`.
- Keep `offset_window` and `offset_to_grain` on derived inputs.

## Field reports

No thread in the catalog communities stated this failure with a stack and a fix that matches the docs. Search [r/dataengineering](https://www.reddit.com/r/dataengineering/) and the tool tags in `sources/catalog.yaml` before treating a comment as a rule.

## Canonical check

- [Creating metrics](https://docs.getdbt.com/docs/build/metrics-overview) — Derived metrics require `expr` and `input_metrics`. Offsets are allowed on derived inputs. The example aliases `order_total` as `revenue` before subtracting.

A derived metric of two simple metrics on the same model can live on that model. If an input comes from another semantic model, the derived metric is top-level.
