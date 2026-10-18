# Layer layout

Raw landing stays a replayable copy of the source. Cleaned models prepare one source concept each. Marts are the only layer built for a consumer, at one declared grain.

## Look for

- `source()` outside staging, or a staging model that joins, aggregates, or filters out source rows.
- Staging grouped by a business domain (`marketing`, `finance`) instead of the source system, so two domains define the same entity.
- A mart, dashboard, or exposure that selects from an intermediate model. Intermediate models are materialized in the main production schema with no extra permission boundary.
- A transform that writes the cleaned or mart table without a persisted raw copy, so a bad model cannot be rebuilt.
- One pipeline that both lands raw files and builds the consumer table, so a transform failure blocks the next ingest.
- Bronze or raw tables that are updated or deleted in place.

## Fix shape

- Land the extract append-only, then build downstream tables from that copy.
- One staging model per source table: rename, cast, and categorize. Materialize staging as views. Keep joins and grain changes in intermediate models.
- Materialize intermediate models as ephemeral, or as views in a schema consumers cannot read. Materialize marts as tables or incremental models, one entity per model, wide enough that a consumer does not rejoin the chain.
- Run ingest and the later layers as separate pipelines.

## Field reports

- [How to handle complex source transformations before a union](https://www.reddit.com/r/dataengineering/comments/1ol5wr5/dbt_how_to_handle_complex_source_transformations/) — r/dataengineering. Staging that filters or derives for one consumer breaks the next model that needs the source rows. The agreed fix is source-faithful staging, grain changes in intermediate models, and a mart as the public model.

## Canonical check

- [dbt staging models](https://docs.getdbt.com/best-practices/how-we-structure/2-staging) — `source()` only in staging, one staging model per source table, no joins and no aggregations. Renames, casts, and calculations that every downstream model needs belong here.
- [dbt intermediate models](https://docs.getdbt.com/best-practices/how-we-structure/3-intermediate) — not exposed to end users. Use them to re-grain or isolate a hard step.
- [dbt marts](https://docs.getdbt.com/best-practices/how-we-structure/4-marts) — one business entity at a declared grain, materialized as a table or incremental model. Start as a view and materialize when the query is too slow.
- [Databricks medallion design](https://docs.databricks.com/aws/en/lakehouse-architecture/deployment-guide/delta-lake) — bronze is the immutable source copy. Silver cleans and integrates. Gold is the business-ready product. Rebuild silver and gold from bronze.
- [Lakeflow medallion guidance](https://docs.databricks.com/aws/en/ldp/best-practices/) — keep the ingest pipeline separate from silver and gold.

The thread's warning against rounding money in staging is the right check when the conversion drops precision. dbt still allows an exact unit conversion in staging when every later model would repeat it. A project that uses the Semantic Layer should keep marts normalized; the wide-mart advice is for projects without that layer.
