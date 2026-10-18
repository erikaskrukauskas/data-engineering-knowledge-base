# Local shuffle reader

`spark.sql.adaptive.localShuffleReader.enabled` stays true so a join that becomes a broadcast does not reread the shuffle over the network.

## Look for

- The flag set to false on a cluster that converts sort-merge joins to broadcast joins.
- A broadcast conversion that is still slow, with this flag off.
- AQE disabled, which makes the flag irrelevant and leaves the sort-merge in place.

## Fix shape

- Leave the flag at its default true when AQE is on.
- Turn AQE back on before tuning the reader.

## Field reports

No thread in the catalog communities stated this failure with a stack and a fix that matches the docs. Search [r/dataengineering](https://www.reddit.com/r/dataengineering/) and the tool tags in `sources/catalog.yaml` before treating a comment as a rule.

## Canonical check

- [Spark SQL performance tuning](https://spark.apache.org/docs/latest/sql-performance-tuning) — The local shuffle reader defaults to true. It applies when AQE is on and a shuffle no longer needs to be re-partitioned, including after a sort-merge join becomes a broadcast hash join.

The reader avoids network traffic for that shuffle. It does not shrink a skewed partition. Skew handling is the other flag.
