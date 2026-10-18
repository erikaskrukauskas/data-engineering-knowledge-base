# Promote environments

Dev, test, and prod use different credentials and different schemas. A change is built in the lower environment before prod.

## Look for

- A laptop `dbt build` against the production target.
- One service principal for dev and prod.
- A deploy that copies files into prod without `dbt build` in the target environment.
- Secrets in the repo instead of a vault per environment.

## Fix shape

- Give each environment a vault and an identity.
- Run `dbt build` in that environment.
- Promote the project from dev to test to prod.

## Field reports

No thread in the catalog communities stated this failure with a stack and a fix that matches the docs. Search [r/dataengineering](https://www.reddit.com/r/dataengineering/) and the tool tags in `sources/catalog.yaml` before treating a comment as a rule.

## Canonical check

- [Microsoft data-heavy design guidance](https://microsoft.github.io/code-with-engineering-playbook/design/design-patterns/data-heavy-design-guidance/) — The playbook separates environments, scans for credentials, and promotes from dev to test to prod.
- [dbt environments](https://docs.getdbt.com/docs/local/dbt-environments) — Targets and profiles are how dbt points a run at one environment.

The playbook is the environment rule. Warehouse object names still come from the custom-schema macro, or dev and prod collide.
