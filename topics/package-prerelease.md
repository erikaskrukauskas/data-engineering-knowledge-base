# Package prereleases

`dbt deps` ignores prerelease versions unless the version pin or `install_prerelease` asks for them.

## Look for

- `install_prerelease: true` in a production `packages.yml`.
- A range that accidentally includes `1.2.0-rc1` because prereleases were enabled.
- A prerelease pin with no ticket to remove it.

## Fix shape

- Pin a release version. Leave `install_prerelease` unset.
- If a prerelease is required, set the version to that exact prerelease rather than a range.

## Field reports

No thread in the catalog communities stated this failure with a stack and a fix that matches the docs. Search [r/dataengineering](https://www.reddit.com/r/dataengineering/) and the tool tags in `sources/catalog.yaml` before treating a comment as a rule.

## Canonical check

- [dbt packages](https://docs.getdbt.com/docs/build/packages) — Prereleases are skipped unless the version string names one or `install_prerelease` is true.

A prerelease can depend on a newer dbt than production runs. The package install succeeding does not mean the project will parse.
