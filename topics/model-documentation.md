# Model documentation

A mart states what one row means, and each column a consumer filters on has a description. A dashboard or application that reads the mart is an exposure with an owner.

## Look for

- A mart YAML entry with tests and no `description`, or a description that only repeats the model name.
- Columns with `data_tests` and no `description`. Warehouse introspection still lists the column in the docs site, so an empty description is easy to miss.
- A dashboard, notebook, or application that selects from a mart with no `exposures` entry, so a change to that model does not show who it breaks.
- An exposure with no `owner` email, or a `type` outside `dashboard`, `notebook`, `analysis`, `ml`, and `application`.
- Docs blocks stored in SQL files. Current dbt looks for them in Markdown files.

## Fix shape

- Add `description` on the mart and on the grain column in the same YAML file as the tests. Use `>` or `|` when the text is longer than a line, or a docs block referenced with `doc()`.
- Declare an exposure for each external consumer: `type`, `owner.email`, and `depends_on` listing the marts it reads.
- Generate the site with `dbt docs generate` on dbt v2 self-hosted. On the dbt platform, Catalog is filled by the job. A `dbt docs generate` step in a platform job does not publish that site.

## Field reports

No thread in the catalog communities stated this failure with a stack and a fix that matches the docs. Search [r/dataengineering](https://www.reddit.com/r/dataengineering/) and the [`dbt` tag](https://stackoverflow.com/questions/tagged/dbt?tab=Votes) for undocumented marts and exposures before treating a comment as a rule.

## Canonical check

- [dbt documentation](https://docs.getdbt.com/docs/build/documentation) — descriptions live in the YAML next to tests. Undocumented columns still appear, with blank text, because dbt reads the warehouse catalog. Docs blocks live in `.md` files and are pulled in with `doc()`.
- [Exposure properties](https://docs.getdbt.com/reference/exposure-properties) — an exposure names a consumer, its type, and the models it depends on. These properties are YAML only. They cannot be set in `config()` or, except for `enabled`, in `dbt_project.yml`.

Descriptions do not enforce a contract. A column can be documented and still change type. The schema-contracts topic is the check that fails the build.
