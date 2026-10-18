# Column masks

A consumer of a mart sees a masked value for a sensitive column. The raw landing copy stays complete so the pipeline can still be replayed.

## Look for

- A mart or a view selected by analysts that returns email, national id, or a similar column with no mask, row filter, or authorized view.
- A policy tag or column mask attached only to a view, on a platform that does not enforce masks on views. BigQuery policy tags apply to tables, not to views.
- Masking implemented by deleting the column from the raw landing table.
- A role that can read the base table granted to the same people who are supposed to see the masked mart.
- One-off `regexp_replace` masks copied into several marts, with no shared function and no test that a non-privileged identity receives the masked value.

## Fix shape

- Keep the raw copy intact. Apply the mask at query time on the table consumers are allowed to read.
- On Databricks, use a Unity Catalog column mask or an ABAC policy when the same rule covers many tables. Use a dynamic view when the consumer must see a joined or reshaped set and must not be granted the base tables.
- On BigQuery, put the policy tag on the table that analysts query, or build an authorized view that selects a masked expression and grant that view instead of the base table.
- Grant an unmasked role only to the identities that operate the pipeline.

## Field reports

- [How do you handle data privacy in BigQuery?](https://www.reddit.com/r/dataengineering/comments/1oy6dn0/how_do_you_handle_data_privacy_in_bigquery/) — r/dataengineering. The team wanted masks on consumption views and found policy tags do not attach to views. The fix that matches the docs is to mask the table analysts actually query, or to publish a view that already applies the expression and is the only object they can read.

## Canonical check

- [Databricks row filters and column masks](https://docs.databricks.com/aws/en/data-governance/unity-catalog/filters-and-masks) — a column mask is a SQL function applied at query time. ABAC policies cover every tagged table. Dynamic views are for a curated projection when the reader must not have the base tables.
- [Microsoft data-tier guidance](https://microsoft.github.io/code-with-engineering-playbook/design/design-patterns/data-heavy-design-guidance/) — bronze remains a copy of the source so a transform can be replayed. Do not drop source fields there to implement a mask.

A comment recommends redacting in the silver layer and handing elevated readers a bypass role. That matches Databricks if those readers are the only ones who can select the base table. It does not match a design that deletes the value from the raw copy.
