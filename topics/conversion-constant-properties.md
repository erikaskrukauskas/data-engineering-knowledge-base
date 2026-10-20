# Conversion constant properties

`constant_properties` lists the dimension or entity that must match between the base event and the conversion event.

## Look for

- A conversion rate counted when the two events share only the entity, even though they must share a campaign or device.
- `constant_properties` pointed at a column that is not a dimension or entity.
- The pair omitted and the window alone used to define a conversion.

## Fix shape

- Add `base_property` and `conversion_property` for each extra match.
- Declare those names as dimensions or entities on the semantic models of the two metrics.

## Field reports

No thread in the catalog communities stated this failure with a stack and a fix that matches the docs. Search [r/dataengineering](https://www.reddit.com/r/dataengineering/) and the tool tags in `sources/catalog.yaml` before treating a comment as a rule.

## Canonical check

- [Creating metrics](https://docs.getdbt.com/docs/build/metrics-overview) — `constant_properties` is optional. Each side names a dimension or entity from the semantic model of that metric.

The entity on the conversion metric is still required. Constant properties add a match. They do not replace the entity.
