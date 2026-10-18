# Git package revisions

A git package sets `revision` to a tag or a 40-character commit. A branch name is not a pin.

## Look for

- `revision: main` or no revision.
- A short SHA.
- A private git URL with the token written in the file instead of `env_var`.

## Fix shape

- Set `revision` to a tag or a full commit hash.
- Supply a token through Jinja in `packages.yml` only.

## Field reports

No thread in the catalog communities stated this failure with a stack and a fix that matches the docs. Search [r/dataengineering](https://www.reddit.com/r/dataengineering/) and the tool tags in `sources/catalog.yaml` before treating a comment as a rule.

## Canonical check

- [dbt packages](https://docs.getdbt.com/docs/build/packages) — The revision is a branch, a tag, or a commit. The documented hash example is 40 characters. `dbt deps` pins the resolved package.

A tag can be moved. A commit hash cannot. Use the hash for a package you do not control.
