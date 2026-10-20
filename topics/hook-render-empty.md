# Hook render on empty runs

A `pre_hook` that uses `source()` or `ref()` calls `.render()` so `--empty` still resolves that relation.

## Look for

- `pre_hook` with `{{ source(...) }}` and a CI job that uses `--empty`.
- An external table refresh hook that fails compilation on an empty run.
- A hook that builds SQL the model never selected, skipped by the empty optimization.

## Fix shape

- Call `.render()` on the `source()` or `ref()` inside the hook.
- Keep the hook SQL valid when dbt skips ordinary relation rendering.

## Field reports

No thread in the catalog communities stated this failure with a stack and a fix that matches the docs. Search [r/dataengineering](https://www.reddit.com/r/dataengineering/) and the tool tags in `sources/catalog.yaml` before treating a comment as a rule.

## Canonical check

- [Hooks](https://docs.getdbt.com/reference/resource-configs/pre-hook-post-hook) — With `--empty`, dbt may skip processing `ref()` and `source()`. `.render()` tells dbt to resolve that relation anyway.

`.render()` does not execute the hook at compile time. It resolves the name so the hook SQL can be built.
