# Environment schemas

A developer run and a pull-request run build into their own schema. Only the production target writes the schema consumers read.

## Look for

- `generate_schema_name` that returns the custom schema alone for every target, so a dev run and production share `marts` or `gold`.
- A pull-request job whose target name is `prod`, or whose `target.schema` is the production schema.
- A CI warehouse user granted write on the production schema.
- A local default target of `prod` in `profiles.yml`.
- Models that set `schema` or `database` to a literal production name inside the SQL file, bypassing the target.

## Fix shape

- Give each person a dev schema (`dbt_<user>`) and each pull request its own schema. Production is a separate target, selected with `--target prod` only on the deploy job.
- Enable `generate_schema_name_for_env` so a custom schema name is used only when `target.name` is `prod`. In dev and CI, keep `target.schema` as a prefix, or use the target schema alone.
- Grant the CI user write on the CI schema and not on the production schema.

## Field reports

- [CI job rebuilt production tables when a pull request was opened](https://www.reddit.com/r/dataengineering/comments/1dt4tik/cicd_dbt_job_is_not_creating_temp_schemas_wtf/) — r/dataengineering. The CI logs showed no temporary schema. The failure was a mis-set target, and the fix the docs support is a schema macro that keeps non-prod runs off the production schema plus a CI role that cannot write production.

## Canonical check

- [Custom schemas](https://docs.getdbt.com/docs/build/custom-schemas) — the default macro prefixes `target.schema` so two environments cannot land on the same relation. `generate_schema_name_for_env` uses the custom schema only when `target.name` is `prod`. The production job must be named `prod` for that macro.
- [Debug schema names](https://docs.getdbt.com/guides/debug-schema-names) — `+schema: silver` becomes `<target.schema>_silver` unless the macro says otherwise. Check the real relation with `dbt ls --output json`.
- [dbt environments](https://docs.getdbt.com/docs/local/dbt-environments) — dev is the default target. Production is an explicit target with its own credentials.

One reply on a schema thread removes the target-schema prefix for every run so folder names match the warehouse. That makes dev, CI, and production write the same schema. The custom-schema page treats that prefix as the collision guard.
