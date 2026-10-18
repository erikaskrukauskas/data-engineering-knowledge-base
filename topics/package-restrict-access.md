# Package access restriction

A package that other projects install sets `restrict-access: true` when only its public models should be referenced.

## Look for

- The default `restrict-access: false`, so a downstream project refs an internal model.
- A public model with no contract.
- A package upgraded and a downstream `ref` that still points at a model the new version made private.

## Fix shape

- Set `restrict-access: true` and mark the supported models `public`.
- Contract those public models.

## Field reports

No thread in the catalog communities stated this failure with a stack and a fix that matches the docs. Search [r/dataengineering](https://www.reddit.com/r/dataengineering/) and the tool tags in `sources/catalog.yaml` before treating a comment as a rule.

## Canonical check

- [access config](https://docs.getdbt.com/reference/resource-configs/access) — `restrict-access` defaults to false. True limits external refs to public models.
- [Model access](https://docs.getdbt.com/docs/mesh/govern/model-access) — Public is the access level another project may ref.

The restriction is enforced at parse time for `ref`. It does not revoke a warehouse grant on the package's schema.
