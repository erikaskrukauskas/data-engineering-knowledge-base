# Unit test struct fields

A unit test that touches a BigQuery STRUCT supplies every field of that STRUCT.

## Look for

- A fixture struct with only the one field the assertion reads.
- A Redshift unit test whose source is in a different database from the models.
- A unit test on a materialized view, recursive SQL, or an introspective query.

## Fix shape

- List every STRUCT field.
- Keep Redshift sources in the same database as the models.
- Do not add a unit test to a materialized view, recursive SQL, or an introspective model.

## Field reports

No thread in the catalog communities stated this failure with a stack and a fix that matches the docs. Search [r/dataengineering](https://www.reddit.com/r/dataengineering/) and the tool tags in `sources/catalog.yaml` before treating a comment as a rule.

## Canonical check

- [Unit tests](https://docs.getdbt.com/docs/build/unit-tests) — BigQuery STRUCT fixtures cannot be a subset of fields. Materialized views, recursive SQL, and introspective queries are not supported. Redshift sources must share the models' database.

The Redshift limitation is called out as a workaround on the unit test page. Read that note for the version you run before copying a Postgres fixture onto Redshift.
