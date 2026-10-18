---
name: review-knowledge-base-sources
description: Recheck the canonical pages and field reports cited by existing data engineering knowledge base topics and update notes whose sources have changed. Use when the user asks to review sources, refresh covered topics, or check whether Reddit, Stack Overflow, or official docs still support topics/.
---

# Review knowledge base sources

Recheck the sources behind topics that already exist. Update a note only when a source contradicts it or adds a check a reviewer can search for. Do not add a new topic; that is the `add-knowledge-base-topic` skill.

## Scope

1. Read `README.md`, `sources/catalog.yaml`, and every file in `topics/`.
2. Review every topic unless the user names one.
3. For each topic, open every link under **Canonical check** and **Field reports**. Follow the catalog entry for that topic when the note's link is incomplete.

## Decide

Compare the live page with **Look for**, **Fix shape**, and **Canonical check**.

Update the note when:

- A canonical page contradicts a bullet.
- A canonical page adds a failure a reviewer can search for in files, and the note omits it.
- The linked thread is gone, or its accepted or highest voted fix changed.
- A version caveat is wrong for the current major version of that tool.

Leave the note unchanged when the pages still support the bullets. Do not refresh subscriber counts or question counts. Do not retarget a tag's `topic` field onto a different note.

If a canonical URL no longer resolves, replace it with the current page for the same document and say that the link moved. If a field report is gone, keep the bullets the canonical page still supports and record the missing URL in the report to the user.

## Edit

Keep the existing headings: the opening rule, **Look for**, **Fix shape**, **Field reports**, **Canonical check**. Summarize source changes. Do not paste long quotations.

Set `captured` in `sources/catalog.yaml` to today's date after the review, including a review that changes nothing.

## Report

For each topic, say `unchanged` or list the bullets that changed and the source URL. Then list any broken links.
