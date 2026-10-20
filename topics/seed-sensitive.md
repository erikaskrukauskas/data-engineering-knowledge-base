# Sensitive seeds

Production data, personal data, and passwords do not live in a seed. The CSV is in git.

## Look for

- A seed of customer emails, account numbers, or passwords.
- A seed described as a production extract.
- A `.csv` in `seeds/` that a reviewer can open and read as personal data.

## Fix shape

- Remove the file from git and load it through the warehouse with the source's access controls.
- Keep a seed only when the rows are non-sensitive reference data.

## Field reports

No thread in the catalog communities stated this failure with a stack and a fix that matches the docs. Search [r/dataengineering](https://www.reddit.com/r/dataengineering/) and the tool tags in `sources/catalog.yaml` before treating a comment as a rule.

## Canonical check

- [Seeds](https://docs.getdbt.com/docs/build/seeds) — The docs list personal data and passwords as poor uses, because the files are version controlled.

A list of test emails to exclude can be a seed. A list of real customer emails should not.
