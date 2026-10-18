# Exposure owners

A downstream dashboard, notebook, or application is an exposure with an owner email and `depends_on`.

## Look for

- A mart with no exposure for the dashboard that selects it.
- An exposure whose owner has a name and no email.
- `depends_on` pointed at a source the dashboard does not query, or missing the mart it does.
- An exposure type outside `dashboard`, `notebook`, `analysis`, `ml`, and `application`.

## Fix shape

- Add the exposure in YAML. Set owner name and email. List the `ref`s the consumer actually uses.
- Only `enabled` can be set from `dbt_project.yml`. The rest belongs on the exposure.

## Field reports

No thread in the catalog communities stated this failure with a stack and a fix that matches the docs. Search [r/dataengineering](https://www.reddit.com/r/dataengineering/) and the tool tags in `sources/catalog.yaml` before treating a comment as a rule.

## Canonical check

- [Exposure properties](https://docs.getdbt.com/reference/exposure-properties) — Exposures are YAML. The type is one of dashboard, notebook, analysis, ml, or application. The owner has a name and an email. `depends_on` lists refs.

An exposure does not run the dashboard or grant it access. It records the dependency so a model change can be traced.
