# Production target name

`generate_schema_name_for_env` uses the custom schema only when `target.name` is `prod`.

## Look for

- A production job whose target is `production`, `prd`, or `live`.
- A CI job whose target is `prod`.
- A developer profile that defaults to `prod`.

## Fix shape

- Name the production target `prod`.
- Name every other target something else, and use the environment-aware schema macro.

## Field reports

No thread in the catalog communities stated this failure with a stack and a fix that matches the docs. Search [r/dataengineering](https://www.reddit.com/r/dataengineering/) and the tool tags in `sources/catalog.yaml` before treating a comment as a rule.

## Canonical check

- [Custom schemas](https://docs.getdbt.com/docs/build/custom-schemas) — `generate_schema_name_for_env` writes the custom schema only when the target name is `prod`. Other targets stay in `target.schema`.

The macro checks the target name, not the warehouse account. A target named `prod` pointed at a development database will write production schema names there.
