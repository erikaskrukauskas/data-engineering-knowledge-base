---
name: add-knowledge-base-topic
description: Add one uncovered topic to the data engineering knowledge base as a checkable failure mode with canonical docs and field reports. Use when the user asks to cover a new topic, add the next topic, expand the knowledge base, or fill a gap in topics/.
---

# Add a knowledge base topic

Add one topic that is not already a file in `topics/`. A topic is a failure mode a reviewer can search for in a project. A field report is a reason to look. A canonical page decides whether a project should change.

## Pick the topic

1. Read `README.md`, `sources/catalog.yaml`, and every file in `topics/`.
2. If the user named a topic, use it. If that topic already has a note, stop and say which file covers it.
3. If the user did not name a topic, cover the first item below that has no note:
   - Schema contracts and data tests
   - Layer layout for raw, cleaned, and mart models
   - Secrets and CI for pipeline repositories
4. When that list is exhausted, pick one gap the existing notes assume and do not check. It must be searchable in files and have an official doc.

Cover one topic, then stop.

## Research

Search official docs before Reddit or Stack Overflow. Match the doc to the tool the topic is about: dbt docs, Airflow docs, Delta Lake, Iceberg, Databricks lakehouse guidance, the Microsoft data-tier guidance, or the Kimball Group techniques.

Then look for one field report in the communities and tags in `sources/catalog.yaml`. Keep a post only when it names the stack, the failure, and a fix, and that fix agrees with the canonical page. Skip career posts, salary posts, "what should I learn" posts, and vendor pitches. The Stack Overflow tag `data-engineering` is too small to mine.

Use a real URL you opened. Do not invent a thread.

## Write the note

Create `topics/<slug>.md`:

```markdown
# <Title>

<One sentence: what must be true after a correct run or review.>

## Look for

- <File, config, or code pattern a reviewer can search for.>

## Fix shape

- <Smallest change that satisfies the canonical page.>

## Field reports

- <Linked thread or tag page, and the failure it shows.>

## Canonical check

<Linked official page> — <the rule the page states, in one or two sentences.>

<When the note must not be applied: adapter differences, table format, or major-version caveats.>
```

Summarize. Do not paste long quotations from docs, books, or threads.

Put disagreed fixes in **Canonical check**, not in **Fix shape**. If no field report qualifies, say so under **Field reports** and point at the tag or community to search later.

## Update the index

- Add one row to the topics table in `README.md`: the title, the note path, and the failure in a few words.
- Add a specific thread under `threads` in `sources/catalog.yaml` with `topic: topics/<slug>.md`.
- Leave a tag's `topic` field as it is when that tag already points at another note. Do not retarget it.
- Leave `captured` and the member or question counts unchanged.

## Report

Tell the user the note path, the canonical page, and the field report. Name the next uncovered queue item without writing it.
