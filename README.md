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
| [Layer layout](topics/layer-layout.md) | Raw stays replayable; only marts face consumers |
| [Secrets and CI](topics/secrets-and-ci.md) | Credentials stay out of git; CI does not build production |
| [Surrogate keys](topics/surrogate-keys.md) | The same grain hashes to the same key on every run |
| [Environment schemas](topics/environment-schemas.md) | Dev and CI do not write the production schema |
| [Failure alerts](topics/failure-alerts.md) | A failed or hung task notifies a person |
| [Partition filters](topics/partition-filters.md) | A large scan names the partition column |
| [Kafka offsets](topics/kafka-offsets.md) | Offsets commit after the landing insert |
| [Column masks](topics/column-masks.md) | Consumers read masked marts; raw stays complete |
| [Model grants](topics/model-grants.md) | The job writes; named roles read |
| [Model documentation](topics/model-documentation.md) | Marts describe their grain and name their consumers |
| [Catchup storms](topics/catchup-storms.md) | A new DAG does not replay every missed interval |
| [Depends on past](topics/depends-on-past.md) | One failed interval does not freeze every later day |
| [Max active runs](topics/max-active-runs.md) | A backfill cannot open an unbounded number of DAG runs |
| [Timestamp zones](topics/timestamp-zones.md) | A business date is the same instant in every stage |
| [Warehouse auto-suspend](topics/warehouse-auto-suspend.md) | A Snowflake warehouse suspends when it is idle |
| [Warehouse size](topics/warehouse-size.md) | A larger Snowflake warehouse is reserved for heavier queries |
| [Delta vacuum](topics/delta-vacuum.md) | VACUUM does not delete files a reader still needs |
| [Spark broadcast joins](topics/spark-broadcast-joins.md) | A broadcast hint is only on a side that fits |
| [Spark skew joins](topics/spark-skew-joins.md) | One hot key does not pin a single task |
| [Iceberg schema evolution](topics/iceberg-schema-evolution.md) | A renamed or reordered column does not move values |
| [Iceberg snapshot expiry](topics/iceberg-expire-snapshots.md) | Old snapshots are expired on purpose |
| [Package pins](topics/package-pins.md) | A dbt package install cannot float to any future major version |
| [Model access](topics/model-access.md) | A private model is not referenced outside its group |
| [Orphaned relations](topics/orphaned-relations.md) | A dropped model does not leave a table consumers still query |
| [Row access policies](topics/row-access-policies.md) | Row filters use the platform policy, not a copied WHERE clause |
| [Alert fan-out](topics/alert-fanout.md) | One failed DAG run sends one notification |
| [Snapshot column rename](topics/snapshot-column-rename.md) | A renamed source column does not fork snapshot history |
| [External Airflow monitoring](topics/external-airflow-monitoring.md) | Airflow is not the only system that notices Airflow is down |
| [Connector flush](topics/connector-flush.md) | A sink commits only after it has flushed a complete batch |
| [Streaming completeness](topics/streaming-completeness.md) | A stream is not treated as a finished batch |
| [Grant clobber](topics/grant-clobber.md) | A model grant adds a role instead of replacing the project list |
| [Public model contracts](topics/public-model-contracts.md) | A public model declares its columns before another project refs it |
| [Freshness is not part of build](topics/freshness-not-in-build.md) | A stale source fails in a step that actually checks freshness |
| [Test severity](topics/test-severity.md) | A broken grain fails the job |
| [Defer state source](topics/defer-state-source.md) | CI compares against the production manifest |
| [CI schema lifetime](topics/ci-schema-drop.md) | A pull-request schema is created for that run and dropped after merge |
| [Dag run timeout](topics/dagrun-timeout.md) | A stuck DAG run does not hold its slot forever |
| [Consecutive DAG failures](topics/consecutive-dag-failures.md) | A DAG that fails every interval is paused on purpose |
| [Task retries](topics/task-retries.md) | A task retries only when the operation is safe to repeat |
| [Task execution timeout](topics/task-execution-timeout.md) | A task that calls out has a timeout |
| [Default pool slots](topics/default-pool-slots.md) | The default pool is not a limit you can ignore on an existing install |
| [Scheduler parallelism](topics/scheduler-parallelism.md) | The scheduler cap is separate from the number of workers |
| [Max active tasks](topics/max-active-tasks.md) | One DAG run cannot take every executor slot |
| [Example DAGs](topics/load-examples.md) | Production does not load Airflow's example DAGs |
| [Rendered field retention](topics/rendered-fields-retention.md) | Rendered templates are kept for the runs you still inspect |
| [Static start date](topics/static-start-date.md) | A DAG start date does not move with the clock |
| [Paused DAG gap](topics/paused-dag-gap.md) | Unpausing a DAG does not replay the paused days |
| [Backfill bounds](topics/backfill-bounds.md) | A backfill names the first and last interval |
| [Deadline alerts](topics/deadline-alerts.md) | A deadline alert is not added to Airflow 2 |
| [SLA miss timing](topics/sla-miss-timing.md) | An SLA callback is not a mid-run alarm |
| [Required environment variables](topics/required-env-vars.md) | A missing dbt environment variable fails the parse |
| [Production target name](topics/prod-target-name.md) | The production job is named prod |
| [Staging source calls](topics/staging-source-only.md) | source() appears only in staging models |
| [Mart grain](topics/mart-grain.md) | A mart is one row per entity |
| [Ephemeral consumers](topics/ephemeral-consumers.md) | A model a tool selects is not ephemeral |
| [Persisted column docs](topics/persist-docs-columns.md) | Warehouse comments are pushed on purpose |
| [Exposure owners](topics/exposure-owners.md) | An exposure names a person and the models it selects |
| [BigQuery grant roles](topics/bigquery-grant-roles.md) | A BigQuery grant uses a role name and a principal prefix |
| [Package prereleases](topics/package-prerelease.md) | Production does not install package prereleases by default |
| [Git package revisions](topics/git-package-revision.md) | A git package pins a tag or a commit |
| [State modified selector](topics/state-modified-selector.md) | CI builds the changed graph, not the whole project |
| [Deferred parents](topics/defer-unselected-parents.md) | CI reuses a production parent only when that parent is not being rebuilt |
| [Freshness thresholds](topics/freshness-thresholds.md) | A freshness check has a count and a period |
| [Loaded-at query](topics/loaded-at-query.md) | A custom freshness clock matches the lookback |
| [Test where clause](topics/test-where-clause.md) | A test's where clause does not hide the failing rows |
| [Stored test failures](topics/store-failures.md) | Stored failures land in a schema the CI user can write |
| [Warehouse minimum charge](topics/warehouse-minimum-charge.md) | A Snowflake warehouse is not resumed for a few seconds |
| [Warehouse query mix](topics/warehouse-query-mix.md) | One warehouse does not run both tiny and huge queries |
| [Resize and running queries](topics/resize-running-queries.md) | A resize does not speed up the query that is already running |
| [Warehouse cache](topics/warehouse-cache.md) | Suspending a warehouse drops its cache |
| [Multi-cluster minimum](topics/multi-cluster-minimum.md) | A multi-cluster warehouse starts extra clusters only when it must |
| [Optimize then vacuum](topics/optimize-then-vacuum.md) | OPTIMIZE does not delete the files it replaced |
| [Delta dataChange](topics/delta-data-change.md) | dataChange is false only when the rows did not change |
| [Replace a table](topics/replace-table.md) | A table is replaced in one command |
| [Iceberg write API](topics/iceberg-write-api.md) | A Spark batch write to Iceberg uses writeTo or MERGE |
| [Iceberg partition evolution](topics/iceberg-partition-evolution.md) | A partition change does not rewrite every old file |
| [Shuffle partitions](topics/shuffle-partitions.md) | A shuffle starts from enough partitions for AQE to coalesce |
| [AQE partition coalesce](topics/aqe-coalesce.md) | Small shuffle partitions are merged when AQE is on |
| [AQE parallelism first](topics/aqe-parallelism-first.md) | A busy cluster does not prefer a flood of tiny tasks |
| [Broadcast timeout](topics/broadcast-timeout.md) | A broadcast join that exceeds 300 seconds fails the query |
| [Skew threshold](topics/skew-threshold.md) | A partition is skewed only when it is both large and unbalanced |
| [Delta log retention](topics/delta-log-retention.md) | The transaction log outlives the data files you still time-travel to |
| [Vacuum lite](topics/vacuum-lite.md) | VACUUM LITE trusts the transaction log |
| [Exactly-once scope](topics/exactly-once-scope.md) | Kafka transactions do not make a database sink exactly-once |
| [Offset commit order](topics/offset-commit-order.md) | The offset moves after the sink write commits |
| [Consumer group reset](topics/consumer-group-reset.md) | A new consumer group does not start at the beginning |
| [Topic retention](topics/topic-retention.md) | Replay stops at the retention window |
| [Partition column functions](topics/partition-function-wrap.md) | A function around a partition column blocks pruning |
| [Partition filters on views](topics/partition-filter-views.md) | A view does not remove the partition filter requirement |
| [Dry-run bytes](topics/dry-run-bytes.md) | A dry run is how a query's scan is checked |
| [Ingest and transform split](topics/ingest-transform-split.md) | The pipeline that lands files is not the pipeline that builds marts |
| [Bronze append](topics/bronze-append.md) | Bronze is not updated in place |
| [Mask function review](topics/mask-function-review.md) | A column mask is a reviewed function, not a copied expression |
| [Package access restriction](topics/package-restrict-access.md) | A package exposes the models it meant to expose |
| [Run-operation access](topics/run-operation-access.md) | A macro is not a way around model access |
| [Snapshot schema](topics/snapshot-schema.md) | Snapshots live in their own schema |
| [dbt clean](topics/dbt-clean-local.md) | dbt clean does not drop warehouse relations |
| [Incremental lookback](topics/incremental-lookback.md) | A lookback window matches how late the source can be |
| [Freshness job step](topics/freshness-checkbox.md) | A freshness checkbox does not fail the build |
| [Uniqueness not enforced](topics/uniqueness-not-enforced.md) | A unique test is not a warehouse constraint on every platform |
| [View contracts](topics/view-contracts.md) | A view contract checks names and types |
| [Template strict undefined](topics/template-strict.md) | A missing Airflow template variable fails the task |
| [Metadata database backup](topics/metadata-backup.md) | The Airflow metadata database is backed up and pruned |
| [Column identity](topics/column-identity.md) | A dropped Iceberg column does not come back under the same name |
| [Expire and retain last](topics/expire-retain-last.md) | Snapshot expiry keeps the recent ancestors you still need |
| [Auto-resume gate](topics/auto-resume-gate.md) | A warehouse with auto-resume off stays stopped until someone starts it |
| [Promote environments](topics/promote-environments.md) | A pipeline is promoted from dev to test to prod |
| [Production service identity](topics/service-principal-prod.md) | The production job does not run as a person |
| [New columns on old rows](topics/new-columns-old-rows.md) | A schema change does not backfill values into existing rows |
| [Local shuffle reader](topics/local-shuffle-reader.md) | AQE reads a shuffle locally when it no longer needs the exchange |

Communities, tags, and thread URLs are in [`sources/catalog.yaml`](sources/catalog.yaml).
