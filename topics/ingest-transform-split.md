# Ingest and transform split

Databricks keeps ingestion in its own pipeline. Silver and gold are downstream.

## Look for

- One job that copies files into bronze and also joins them into a mart, so a transform failure reruns the ingest.
- A bronze table updated in place.
- A gold table built by reading raw files instead of the bronze table.

## Fix shape

- Land bronze in an ingest pipeline.
- Build silver and gold from bronze tables.
- Append to bronze. Replay from bronze when the transform changes.

## Field reports

No thread in the catalog communities stated this failure with a stack and a fix that matches the docs. Search [r/dataengineering](https://www.reddit.com/r/dataengineering/) and the tool tags in `sources/catalog.yaml` before treating a comment as a rule.

## Canonical check

- [Delta Lake deployment](https://docs.databricks.com/aws/en/lakehouse-architecture/deployment-guide/delta-lake) — Separate the ingest pipeline from the silver and gold pipelines. Bronze stays available to replay.

The layer-layout note is the dbt staging version of this split. This note is the Databricks pipeline boundary.
