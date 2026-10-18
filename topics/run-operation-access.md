# Run-operation access

From dbt v1.12, `dbt run-operation` does not enforce model access. Do not use it to read a private model from outside the group.

## Look for

- A macro that `ref`s a private model from another group's job.
- A run-operation scheduled in production because the selector refused the private model.
- An older dbt where the same macro failed and was patched by disabling access instead of changing the model to public.

## Fix shape

- Make the model public if another group must build on it.
- Keep the macro in the owning group if the model stays private.

## Field reports

No thread in the catalog communities stated this failure with a stack and a fix that matches the docs. Search [r/dataengineering](https://www.reddit.com/r/dataengineering/) and the tool tags in `sources/catalog.yaml` before treating a comment as a rule.

## Canonical check

- [Model access](https://docs.getdbt.com/docs/mesh/govern/model-access) — v1.12 and later do not enforce access inside `dbt run-operation`.

On a dbt older than 1.12, check that version's access page before assuming run-operation is exempt. The current docs state the exemption for v1.12+.
