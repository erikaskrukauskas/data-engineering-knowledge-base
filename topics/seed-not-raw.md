# Seeds are not raw loads

Seeds are versioned CSV files of business mappings. Raw extracts and large files are loaded by another tool.

## Look for

- A multi-gigabyte CSV in `seeds/`.
- `dbt seed` as the ingestion step for a production table.
- A seed path left at `seeds/` while the files were moved, with no `seed-paths` change.

## Fix shape

- Keep seeds small and slow-changing.
- Load raw files with the warehouse load tool.
- Set `seed-paths` if the directory is not `seeds`.

## Field reports

No thread in the catalog communities stated this failure with a stack and a fix that matches the docs. Search [r/dataengineering](https://www.reddit.com/r/dataengineering/) and the tool tags in `sources/catalog.yaml` before treating a comment as a rule.

## Canonical check

- [Seeds](https://docs.getdbt.com/docs/build/seeds) — Seeds suit mappings such as country codes. They are a poor fit for raw exports and large files. Loading large CSVs this way is not performant.

`dbt seed` truncates and reinserts. It is not an incremental load. A large file will be fully rewritten on every seed run.
