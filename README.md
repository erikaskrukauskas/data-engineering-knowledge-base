# Data engineering knowledge base

Field reports from Reddit and Stack Overflow. Each topic is a failure mode a reviewer can check. A thread becomes a reason to look, and an official doc decides whether a project should change.

Captured 2026-10-17. Question counts and subscriber counts move; the links are the stable part.

## How to use a topic

1. Read the topic note and confirm the project uses that stack.
2. Search the files named in **Look for**.
3. Cite the field report as a known failure. Cite the canonical page before recommending a change.
4. Skip career posts, salary threads, "what should I learn" posts, and vendor pitches.

`r/dataengineering` is the noisiest source. Keep a post when it names the stack, the volume or schedule, and what broke on retry, backfill, or late data.

The Stack Overflow tag [`data-engineering`](https://stackoverflow.com/questions/tagged/data-engineering) has only about 60 questions. Use the tool tags in `sources/catalog.yaml`.

## Topics

| Topic | Field report |
|---|---|
| [Idempotent loads](topics/idempotent-loads.md) | Retrying a task must not duplicate rows |
| [Incremental loads and backfill](topics/incremental-and-backfill.md) | Rebuilding a past window must not duplicate rows |
| [Deletes and late data](topics/deletes-and-late-data.md) | An empty extract must not wipe the target |
| [Airflow orchestration](topics/airflow-orchestration.md) | The worker orchestrates; it does not hold the dataset |
| [Airflow and dbt](topics/airflow-and-dbt.md) | Ingestion and SQL transforms stay separately parameterized |
| [Spark and table layout](topics/spark-layout.md) | Partition and file size come from the table format docs |
| [Schema contracts and data tests](topics/schema-contracts-and-data-tests.md) | Grain, column types, and freshness fail the run |

Communities, tags, and thread URLs are in [`sources/catalog.yaml`](sources/catalog.yaml).
