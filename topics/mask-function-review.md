# Mask function review

The mask function is owned, tested, and attached as a Unity Catalog mask or an ABAC policy.

## Look for

- A `regexp_replace` pasted into every mart.
- A different function name for the same rule on each table.
- Users granted `SELECT` on the base table and also on the masked view, so they bypass the mask.

## Fix shape

- One function for the rule.
- Attach it as a column mask, or as an ABAC policy when the same tag applies to many tables.
- Grant consumers the masked relation, not the base table.

## Field reports

No thread in the catalog communities stated this failure with a stack and a fix that matches the docs. Search [r/dataengineering](https://www.reddit.com/r/dataengineering/) and the tool tags in `sources/catalog.yaml` before treating a comment as a rule.

## Canonical check

- [Databricks filters and masks](https://docs.databricks.com/aws/en/data-governance/unity-catalog/filters-and-masks) — Column masks and ABAC policies are Unity Catalog functions applied at query time. A policy at catalog or schema scope covers tagged tables.

BigQuery policy tags do not attach to views. A view in that project needs its own protection. The column-masks note holds that case.
