# Metric names

A metric `name` is unique and uses lowercase letters, numbers, and underscores.

## Look for

- Two metrics named `revenue` in different YAML files.
- A name with spaces or capitals.
- A label copied into `name`. The label is the display string and may contain spaces.

## Fix shape

- Give each metric one lowercase name.
- Put the human title in `label`.

## Field reports

No thread in the catalog communities stated this failure with a stack and a fix that matches the docs. Search [r/dataengineering](https://www.reddit.com/r/dataengineering/) and the tool tags in `sources/catalog.yaml` before treating a comment as a rule.

## Canonical check

- [Creating metrics](https://docs.getdbt.com/docs/build/metrics-overview) — `name` is required, unique, and limited to lowercase letters, numbers, and underscores. `label` is the display value and may contain spaces.

Uniqueness is across the semantic layer, not per file. Two YAML files can still collide.
