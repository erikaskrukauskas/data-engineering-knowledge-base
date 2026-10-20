# Conversion metric window

A conversion metric sets `entity`, `calculation`, `base_metric`, and `conversion_metric`. The window is the time allowed between the two events.

## Look for

- `type: conversion` missing `entity` or `calculation`.
- `calculation` set to a value other than `conversion_rate` or `conversions`.
- A window omitted when the business rule is seven days, so the metric uses whatever default the query happens to apply.
- A cross-model conversion nested on one model.

## Fix shape

- Set the four required keys.
- Set `window` to the allowed gap.
- Put `constant_properties` only when the base and conversion events must match on an extra dimension.

## Field reports

No thread in the catalog communities stated this failure with a stack and a fix that matches the docs. Search [r/dataengineering](https://www.reddit.com/r/dataengineering/) and the tool tags in `sources/catalog.yaml` before treating a comment as a rule.

## Canonical check

- [Creating metrics](https://docs.getdbt.com/docs/build/metrics-overview) — Conversion metrics require entity, calculation, base metric, and conversion metric. `window` is the optional time bound. Cross-model conversions are top-level.

`constant_properties` refer to dimensions or entities of the semantic models linked to each metric. They are not arbitrary SQL.
