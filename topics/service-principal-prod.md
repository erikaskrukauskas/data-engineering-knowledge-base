# Production service identity

The production writer is a service identity. People receive select, not the job's credentials.

## Look for

- A personal user in the production profile.
- Developers granted the production role so a laptop can rebuild a mart.
- A CI identity that can write the production schema.
- A key file committed next to `profiles.yml`.

## Fix shape

- Run prod as a service principal whose rights are the prod schemas.
- Grant people select on the marts.
- Keep the CI identity on the CI schema.

## Field reports

No thread in the catalog communities stated this failure with a stack and a fix that matches the docs. Search [r/dataengineering](https://www.reddit.com/r/dataengineering/) and the tool tags in `sources/catalog.yaml` before treating a comment as a rule.

## Canonical check

- [dbt grants](https://docs.getdbt.com/reference/resource-configs/grants) — Grants are how the built relation is shared. The building identity and the reading identity are different lists.
- [Microsoft data-heavy design guidance](https://microsoft.github.io/code-with-engineering-playbook/design/design-patterns/data-heavy-design-guidance/) — Credentials are per environment and are not stored in the project.

A grant to a personal user can be what an analyst needs. It is not what the production job should use as its profile user.
