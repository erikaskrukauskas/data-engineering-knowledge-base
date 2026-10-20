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
| [Semantic aggregation time](topics/semantic-agg-time.md) | A semantic model names its aggregation time dimension |
| [Semantic model names](topics/semantic-name-underscores.md) | A semantic model name does not contain a double underscore |
| [Semantic primary entity](topics/semantic-primary-entity.md) | Every semantic dimension is tied to one entity |
| [Time dimension granularity](topics/semantic-time-granularity.md) | A time dimension declares the grain of the column |
| [Derived semantic expressions](topics/semantic-derived-expr.md) | A derived dimension or entity has an expr |
| [Metric YAML placement](topics/semantic-metric-placement.md) | A simple metric stays on its model, and a cross-model metric stays at the top level |
| [Metric filter selection](topics/semantic-filter-selection.md) | A filter-only use of a semantic model is not a dbt selection edge |
| [Dimension name scope](topics/semantic-dimension-scope.md) | Dimension names are unique for one primary entity |
| [Simple metric aggregation](topics/simple-metric-agg.md) | A simple metric names an aggregation |
| [Cumulative time spine](topics/cumulative-time-spine.md) | A cumulative metric has a time spine before it is queried |
| [Non-additive balances](topics/non-additive-balance.md) | A balance is not summed across time |
| [Metric rollup grain](topics/metric-rollup-grain.md) | A query does not roll a metric finer than the column grain |
| [Ratio filter scope](topics/ratio-filter-scope.md) | A ratio filter is applied to the side that should be filtered |
| [Derived metric aliases](topics/derived-metric-alias.md) | A derived expression uses the aliases of its input metrics |
| [Conversion metric window](topics/conversion-metric-window.md) | A conversion metric names the entity, the two events, and the window |
| [Metric filter syntax](topics/metric-filter-syntax.md) | A metric filter uses the MetricFlow template, not raw SQL |
| [Unit test file path](topics/unit-test-yaml-path.md) | A unit test is defined under the models directory |
| [Unit tests in production](topics/unit-tests-in-prod.md) | Production builds do not spend a warehouse on static unit tests |
| [Unit test inputs](topics/unit-test-missing-input.md) | Every ref and source in a unit-tested model is an input |
| [Unit test join aliases](topics/unit-test-join-alias.md) | Joined tables in a unit-tested model are aliased |
| [Incremental unit test expectation](topics/unit-test-incremental-expect.md) | An incremental unit test expects the rows to merge, not the table after the merge |
| [Ephemeral unit test input](topics/unit-test-ephemeral-input.md) | An ephemeral parent is mocked as SQL |
| [Unit test struct fields](topics/unit-test-struct-fields.md) | A BigQuery STRUCT fixture lists every field |
| [Local unit test compute](topics/unit-test-local-compute.md) | Local unit tests are opt-in and only where DuckDB can translate the SQL |
| [Empty parents for unit tests](topics/unit-test-empty-parents.md) | Unit test parents exist before the test runs |
| [SQL unit test columns](topics/unit-test-sql-columns.md) | A SQL unit-test fixture includes every column |
| [Versioned model unit tests](topics/unit-test-versioned-models.md) | A unit test on a versioned model runs on each version unless you say otherwise |
| [Sensor poke mode](topics/sensor-poke-mode.md) | A long sensor wait does not hold a worker slot |
| [Sensor reschedule interval](topics/sensor-reschedule-interval.md) | A reschedule sensor waits at least a minute between checks |
| [Sensor soft fail](topics/sensor-soft-fail.md) | A sensor timeout fails the task unless skip is the business rule |
| [Sensor timeout clock](topics/sensor-timeout-clock.md) | A sensor timeout counts from the first poke, including reschedule delays |
| [Deferrable sensors](topics/deferrable-sensor.md) | A long wait defers to the triggerer instead of occupying a worker |
| [Delta change feed enablement](topics/delta-cdf-enable.md) | Change data feed is on before a reader asks for changes |
| [Change feed retention](topics/delta-cdf-vacuum.md) | VACUUM does not delete change data a reader still needs |
| [Change feed and column mapping](topics/delta-cdf-column-mapping.md) | A change-feed read does not span a non-additive schema change |
| [Change feed stream start](topics/delta-cdf-stream-start.md) | A CDF stream names the version where changes begin |
| [Seeds are not raw loads](topics/seed-not-raw.md) | A seed is a small static mapping, not a CSV extract |
| [Sensitive seeds](topics/seed-sensitive.md) | A seed file does not hold passwords or personal data |
| [Seed column changes](topics/seed-column-refresh.md) | A changed seed header is loaded with a full refresh |
| [Seed column types](topics/seed-column-types.md) | A seed column that is not a number is given an explicit type |
| [Hooks inside a transaction](topics/hook-inside-transaction.md) | A statement that cannot run inside a transaction is marked that way |
| [Hook order](topics/hook-order.md) | Package hooks run before project hooks, and model hooks run last |
| [Hook render on empty runs](topics/hook-render-empty.md) | A hook that references a relation calls render when the run may be empty |
| [Stream offset advance](topics/stream-offset-advance.md) | A Snowflake stream advances only when a DML statement consumes it |
| [Stream staleness](topics/stream-staleness.md) | A stream is consumed before its stale-after time |
| [Stream has-data checks](topics/stream-has-data-tasks.md) | A true stream-has-data result is consumed even when the select is empty |
| [One stream per consumer](topics/stream-one-consumer.md) | Two jobs do not share one Snowflake stream |
| [Standard stream net changes](topics/stream-standard-delta.md) | A standard stream does not return a row that was inserted and then deleted |
| [View change tracking](topics/stream-view-tracking.md) | A stream on a view has change tracking on the view and its tables |
| [Stream schema changes](topics/stream-schema-change.md) | A source schema change is compatible with the stream offset |
| [Replacing a streamed table](topics/stream-replace-source.md) | CREATE OR REPLACE on a streamed table is a new history |
| [Append-only stream deletes](topics/stream-append-only-deletes.md) | An append-only stream keeps inserts that were later deleted |
| [Insert-only external streams](topics/stream-insert-only-external.md) | An external insert-only stream does not report a removed file |
| [Clustering column order](topics/cluster-column-order.md) | The first clustering column is the one queries filter first |
| [Clustering existing data](topics/cluster-existing-data.md) | Altering a table to add clustering does not rewrite old blocks |
| [Clustering small tables](topics/cluster-small-tables.md) | Clustering is for tables and partitions large enough to have blocks |
| [Clustering and long-term storage](topics/cluster-storage-pricing.md) | An unpartitioned table ages into long-term storage only when the whole table is quiet |
| [Streaming watermark](topics/streaming-watermark.md) | A windowed stream declares how late an event may be |
| [Watermark output mode](topics/watermark-output-mode.md) | Append mode waits for the watermark before it emits a window |
| [Stream-stream joins](topics/stream-stream-join.md) | An outer or semi stream join has watermarks and a time condition |
| [Session window limits](topics/session-window-limits.md) | A streaming session window is not written in update mode |
| [Streaming checkpoint](topics/streaming-checkpoint.md) | A fault-tolerant stream writes its checkpoint to durable storage |
| [Stateful query split](topics/stateful-query-split.md) | A streaming query keeps one stateful operation |
| [Seed downstream builds](topics/seed-downstream-build.md) | A seed is selected by name when downstream models must rerun |
| [Change feed row images](topics/cdf-change-types.md) | An update in the change feed is a preimage and a postimage |
| [Audit hook rollback](topics/hook-audit-rollback.md) | An audit row that must survive a failed model is written outside the transaction |
| [Metric names](topics/metric-name-shape.md) | A metric name is unique and lowercase |
| [Semantic entity joins](topics/semantic-entity-join.md) | Entities with the same name are the join keys |
| [Unit test exit codes](topics/unit-test-exit-code.md) | A unit test failure is one failure, not a count of bad rows |
| [Sensor backoff](topics/sensor-exponential-backoff.md) | A flaky remote check backs off instead of poking on a fixed short interval |
| [Change feed writer version](topics/cdf-writer-version.md) | A table with change data feed is not written by Delta 1.2.1 or older |
| [Clustering block pruning](topics/cluster-block-pruning.md) | A clustered query filters the clustered columns without wrapping them |
| [Late data bound](topics/streaming-late-data-bound.md) | A watermark delay is the lateness the product will still count |
| [Seed tests](topics/seed-properties.md) | A seed that models ref has the same grain tests as a model |
| [Stream metadata columns](topics/stream-metadata-columns.md) | A stream consumer reads the metadata columns, not only the payload |
| [Stream stale-after visibility](topics/stream-stale-after-role.md) | The role that checks staleness can see the source retention |
| [Streams on shared tables](topics/stream-shared-retention.md) | A stream on a shared table does not extend the provider's retention |
| [Clustering SQL dialect](topics/cluster-sql-dialect.md) | Clustered tables are queried and written with GoogleSQL |
| [Reclustering and slots](topics/cluster-recluster-capacity.md) | Automatic reclustering does not add query slots |
| [Streaming output modes](topics/streaming-output-matrix.md) | The output mode matches the query shape |
| [Chained window time](topics/streaming-window-time.md) | A second window is applied to the window time, not to the window struct |
| [Streaming source replay](topics/streaming-source-replay.md) | A restarted stream can replay only a source that supports it |
| [State store locality](topics/state-store-locality.md) | Streaming state stays on the executor that already loaded it |
| [RocksDB state memory](topics/rocksdb-memory-limit.md) | A RocksDB memory cap is a soft limit |
| [Multiple hook statements](topics/hook-multiple-statements.md) | Several hook statements are a list, in order |
| [Selecting one seed](topics/seed-one-at-a-time.md) | A seed command can load one file |
| [Change feed version bounds](topics/cdf-version-bounds.md) | A change-feed batch read is inclusive of the start and end |
| [Change data folder](topics/cdf-empty-folder.md) | An empty change-data folder does not mean the feed is off |
| [Unit test macro overrides](topics/unit-test-macro-overrides.md) | A unit test sets the macro and variable values the branch needs |
| [File sensor mode](topics/file-sensor-defer.md) | A file wait uses the file sensor and a mode that releases the worker |
| [Time spine nulls](topics/fill-nulls-timespine.md) | A metric that must show zeros joins the time spine and fills nulls |
| [One semantic model per dbt model](topics/semantic-one-model.md) | A dbt model defines one semantic model |
| [Conversion constant properties](topics/conversion-constant-properties.md) | A conversion matches on the properties that define the same subject |
| [Complete mode state](topics/streaming-complete-mode-state.md) | Complete mode keeps every aggregate |
| [Post-hook statistics](topics/hook-spark-analyze.md) | A Spark table that needs statistics gets them in a post-hook |
| [Seed full refresh cascade](topics/seed-drop-cascade.md) | A seed full refresh drops objects that depend on the seed table |
| [Stream transaction lock](topics/stream-explicit-transaction.md) | Statements that must see one stream delta share a transaction |
| [First cluster column filters](topics/cluster-filter-first-column.md) | Queries filter the leading cluster column when that is how the table was ordered |
| [Watermark cleanup conditions](topics/streaming-watermark-cleanup-conditions.md) | State cleanup requires append or update, the right column, and the call before the aggregation |
| [Unit tests stay in the project](topics/unit-test-current-project.md) | A unit test targets a SQL model in the current project |

Communities, tags, and thread URLs are in [`sources/catalog.yaml`](sources/catalog.yaml).
