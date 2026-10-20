# Local unit test compute

`compute: local` runs a unit test on DuckDB. It is experimental, limited to Snowflake and BigQuery, and it fails closed when translation fails.

## Look for

- `compute: local` without `DBT_ENGINE_EXPERIMENTAL_LOCAL_UNIT_TESTS=true`.
- A local unit test on a platform other than Snowflake or BigQuery.
- `static_analysis: off` on a test that also sets `compute: local`.
- A warehouse function such as `haversine` or `AI_CLASSIFY` with no DuckDB equivalent.

## Fix shape

- Set the environment variable before using local compute.
- Leave `compute: remote` when the SQL cannot be translated.
- Do not expect local mode to fall back to the warehouse.

## Field reports

No thread in the catalog communities stated this failure with a stack and a fix that matches the docs. Search [r/dataengineering](https://www.reddit.com/r/dataengineering/) and the tool tags in `sources/catalog.yaml` before treating a comment as a rule.

## Canonical check

- [Unit tests](https://docs.getdbt.com/docs/build/unit-tests) — Local unit tests are experimental. Without the environment variable, dbt reports an invalid configuration. Translation failure is a test failure. `local` does not fall back to the warehouse.

Upstream schemas are fetched once and cached. If those relations do not exist yet, the local test fails while fetching the schema. Build them with `--empty` first.
