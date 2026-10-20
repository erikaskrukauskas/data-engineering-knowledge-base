# Multiple hook statements

A hook with more than one statement is a list. A single string is one statement. The list runs in order.

## Look for

- Two SQL statements concatenated in one string.
- A list used where one statement was intended, so an extra statement runs.
- A macro that returns SQL called without the extra braces when it needs `ref()` at execution time.

## Fix shape

- Pass a list of statements.
- Wrap a macro that depends on execution-time values in the extra curly braces the hook page shows.

## Field reports

No thread in the catalog communities stated this failure with a stack and a fix that matches the docs. Search [r/dataengineering](https://www.reddit.com/r/dataengineering/) and the tool tags in `sources/catalog.yaml` before treating a comment as a rule.

## Canonical check

- [Hooks](https://docs.getdbt.com/reference/resource-configs/pre-hook-post-hook) — A string is one statement. A list is several, run in definition order. A macro that needs execution-time values such as `ref()` is wrapped so those values resolve.

Package hooks still run before these statements. The list order is only inside that hook context.
