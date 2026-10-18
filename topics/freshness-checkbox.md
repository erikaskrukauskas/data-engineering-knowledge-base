# Freshness job step

The job step that must stop the pipeline is a `dbt freshness` command, not the source-freshness checkbox.

## Look for

- The platform checkbox enabled and `dbt build` after it, with a stale source still building.
- No `dbt freshness` command in the job at all.
- Freshness compared to a non-production manifest.

## Fix shape

- Add a command step that runs `dbt freshness` before `dbt build`.
- Let that command's failure stop the job.

## Field reports

No thread in the catalog communities stated this failure with a stack and a fix that matches the docs. Search [r/dataengineering](https://www.reddit.com/r/dataengineering/) and the tool tags in `sources/catalog.yaml` before treating a comment as a rule.

## Canonical check

- [Source freshness jobs](https://docs.getdbt.com/docs/deploy/source-freshness) — `dbt build` does not run freshness. A command step fails the job. The checkbox does not stop the later steps.

The command is `dbt freshness`. `dbt source freshness` is the legacy form.
