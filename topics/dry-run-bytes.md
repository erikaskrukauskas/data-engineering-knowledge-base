# Dry-run bytes

A BigQuery dry run estimates bytes before a job is submitted. It is not billed as a slot query.

## Look for

- A new dashboard query run against production to see if it is expensive.
- `LIMIT 10` used as the cost check.
- A partition filter added after the expensive query already ran.

## Fix shape

- Dry-run the query.
- Add the partition predicate the dry run says is missing, then run it.

## Field reports

No thread in the catalog communities stated this failure with a stack and a fix that matches the docs. Search [r/dataengineering](https://www.reddit.com/r/dataengineering/) and the tool tags in `sources/catalog.yaml` before treating a comment as a rule.

## Canonical check

- [BigQuery compute best practices](https://cloud.google.com/bigquery/docs/best-practices-performance-compute) — Dry runs report the bytes the query would scan and are not billed as query jobs. LIMIT does not change that scan.

A dry run estimates the scan. It does not execute the query or populate the destination.
