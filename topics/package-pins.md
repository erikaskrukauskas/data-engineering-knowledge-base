# Package pins

Hub packages declare a version range inside one minor release. Git packages declare a revision.

## Look for

- A Hub package with no `version`. Hub packages require one.
- A range such as `>=0.7.0` with no upper bound.
- A git package with no `revision`, or a revision that is a branch name everyone keeps pushing.
- `install_prerelease: true` on a production project.
- A git token rendered with Jinja inside `dependencies.yml`. That file does not render Jinja. The token method belongs in `packages.yml`.

## Fix shape

- Pin a Hub package to the latest patch of one minor, for example `">=0.7.0", "<0.8.0"`.
- Pin a git package to a tag or a full commit hash.
- Let `dbt deps` write the lock it already pins by default. Commit that lock.

## Field reports

No thread in the catalog communities stated this failure with a stack and a fix that matches the docs. Search [r/dataengineering](https://www.reddit.com/r/dataengineering/) and the tool tags in `sources/catalog.yaml` before treating a comment as a rule.

## Canonical check

- [dbt packages](https://docs.getdbt.com/docs/build/packages) — Hub packages require a version. dbt recommends a range bounded to one minor. `dbt deps` pins packages by default and does not select prereleases unless the version says so or `install_prerelease` is true.
- [Project dependencies](https://docs.getdbt.com/docs/mesh/govern/project-dependencies) — `dependencies.yml` does not render Jinja. A git token in the package spec stays in `packages.yml`.

A package's models run as part of your project. Pinning the version does not stop a package model from landing in your schema. Access rules for package models are a separate config.
