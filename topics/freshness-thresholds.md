# Freshness thresholds

`warn_after` and `error_after` each set `count` and `period`. A source with neither is not checked.

## Look for

- `freshness:` present with only `loaded_at_field`.
- `error_after` with a count and no period.
- A period of `day` on a source that arrives hourly, used as an hourly SLA.

## Fix shape

- Set `loaded_at_field` or `loaded_at_query`.
- Set `warn_after` and `error_after` with both `count` and `period`.

## Field reports

No thread in the catalog communities stated this failure with a stack and a fix that matches the docs. Search [r/dataengineering](https://www.reddit.com/r/dataengineering/) and the tool tags in `sources/catalog.yaml` before treating a comment as a rule.

## Canonical check

- [Freshness config](https://docs.getdbt.com/reference/resource-configs/freshness) — dbt skips the check when neither threshold is set. Each threshold has a count and a period.

Configuring freshness does not run it. The job still has to call `dbt freshness`.
