# Bronze append

The raw table is an append of what arrived. A correction is a new record or a downstream change, not an edit of the landed file.

## Look for

- `UPDATE` or `DELETE` on bronze to fix a transform bug.
- A job that overwrites yesterday's bronze partition because the silver model was wrong.
- PII deleted from bronze as the masking strategy.

## Fix shape

- Append the source payload.
- Fix the transform in silver or gold and replay from bronze.
- Mask in the consumer layer.

## Field reports

No thread in the catalog communities stated this failure with a stack and a fix that matches the docs. Search [r/dataengineering](https://www.reddit.com/r/dataengineering/) and the tool tags in `sources/catalog.yaml` before treating a comment as a rule.

## Canonical check

- [Delta Lake deployment](https://docs.databricks.com/aws/en/lakehouse-architecture/deployment-guide/delta-lake) — Bronze is the replay source. The deployment guide separates it from the tables that apply business rules.
- [Databricks column masks](https://docs.databricks.com/aws/en/data-governance/unity-catalog/filters-and-masks) — Masking is a query-time function. It is not a delete of the source column.

An actual legal deletion is a different job, with a record of what was removed. It is not how a transform bug gets fixed.
