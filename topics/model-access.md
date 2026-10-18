# Model access

Models that are implementation details are private to their group. Models other projects may `ref` are public.

## Look for

- Every model left at the default `protected`, including staging models that other groups select.
- A mart marked `private` that a dashboard project is expected to `ref`.
- A package with `restrict-access: false` (the default), so the root project can `ref` the package's private models.
- `dbt run-operation` used to bypass access. From v1.12, run-operation does not enforce access.

## Fix shape

- Set `access: private` on staging and intermediate models in a group. Set `access: public` on marts other projects use, and rerun the production job so the change is applied.
- Set `restrict-access: true` in a package that should expose only public models.

## Field reports

No thread in the catalog communities stated this failure with a stack and a fix that matches the docs. Search [r/dataengineering](https://www.reddit.com/r/dataengineering/) and the tool tags in `sources/catalog.yaml` before treating a comment as a rule.

## Canonical check

- [Model access](https://docs.getdbt.com/docs/mesh/govern/model-access) — Private is the same group. Protected is the same project. Public is any group, package, or project. A `ref` outside that scope fails at parse time.
- [access config](https://docs.getdbt.com/reference/resource-configs/access) — The default is protected. Package access restrictions are off unless the package sets `restrict-access: true`.

Access is not a warehouse grant. A private model can still be selected by anyone who has SQL access to the schema. Grants are the other topic.
