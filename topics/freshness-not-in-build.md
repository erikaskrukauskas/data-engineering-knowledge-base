# Freshness is not part of build

`dbt build` does not run freshness. A job that must stop on stale inputs runs `dbt freshness` where a failure should stop the later steps.

## Look for

- A production job that is only `dbt build`, with `freshness` configured and never executed.
- Sources with `loaded_at_field` and no `warn_after` or `error_after`. dbt will not check them.
- A freshness selector pointed at a non-production manifest.

## Fix shape

- Set `warn_after` or `error_after` with both `count` and `period`.
- Run `dbt freshness` as its own step. On the platform, a checkbox that runs freshness first does not fail the later steps. A command step does.

## Field reports

No thread in the catalog communities stated this failure with a stack and a fix that matches the docs. Search [r/dataengineering](https://www.reddit.com/r/dataengineering/) and the tool tags in `sources/catalog.yaml` before treating a comment as a rule.

## Canonical check

- [Freshness config](https://docs.getdbt.com/reference/resource-configs/freshness) — Without `warn_after` or `error_after`, dbt does not check that resource. `dbt source freshness` is the legacy command. `dbt freshness` is the current one.
- [Source freshness jobs](https://docs.getdbt.com/docs/deploy/source-freshness) — `dbt build` does not include freshness. A command step fails the job. The checkbox does not.

Check frequency has to be shorter than the SLA. A daily check cannot enforce a one-hour SLA. The deploy page suggests at least twice the SLA frequency.
