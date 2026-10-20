# Hook order

Hooks from packages run first, then hooks from `dbt_project.yml`, then hooks defined on the model, each group in the order written.

## Look for

- A model `pre_hook` assumed to run before a package hook that prepares the session.
- Two project hooks written in the wrong order, such as a grant before the supporting role exists.
- A hook in both the project file and the model, with one of them deleted in the belief that they override.

## Fix shape

- Put the package setup in the package.
- Order project hooks so each statement sees the previous one.
- Keep both lists when both should run. They accumulate.

## Field reports

No thread in the catalog communities stated this failure with a stack and a fix that matches the docs. Search [r/dataengineering](https://www.reddit.com/r/dataengineering/) and the tool tags in `sources/catalog.yaml` before treating a comment as a rule.

## Canonical check

- [Hooks](https://docs.getdbt.com/reference/resource-configs/pre-hook-post-hook) — Package hooks run before the active package. Model hooks run after project hooks. Hooks in one context run in definition order. Project and model hooks both apply.

Seeds and snapshots take the same hook configs. A model-only mental model misses a seed hook that runs in the seed build.
