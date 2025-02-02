# StarRocks version 2.1

## 2.1.0

Release date: February 24, 2022

### New Features

- [Preview] StarRocks now supports Iceberg external tables.
- [Preview] The pipeline engine is now available. It is a new execution engine designed for multicore scheduling. The query parallelism can be adaptively adjusted without the need to set the parallel_fragment_exec_instance_num parameter. This also improves performance in high concurrency scenarios.
- The CTAS (CREATE TABLE AS SELECT) statement is supported, making ETL and table creation easier.
- SQL fingerprint is supported. SQL fingerprint is generated in audit.log, which facilitates the location of slow queries.
- The ANY_VALUE, ARRAY_REMOVE and SHA2 functions are supported.

### Improvements

- Compaction is optimized. A flat table can contain up to 10,000 columns.
- The performance of first-time scan and page cache is optimized. Random I/O is reduced to improve first-time scan performance. The improvement is more noticeable if first-time scan occurs on SATA disks. StarRocks' page cache can store original data, which eliminates the need for bitshuffle encoding and unnecessary decoding. This improves the cache hit rate and query efficiency.
- Schema change is supported in the primary key model. You can add, delete, and modify bitmap indexes by using `Alter table`.
- [Preview] The size of a string can be up to 1 MB.
- JSON load performance is optimized. You can load more than 100 MB JSON data in a single file.
- Bitmap index performance is optimized.
- The performance of StarRocks Hive external tables is optimized. Data in the CSV format can be read.
- DEFAULT CURRENT_TIMESTAMP is supported in the `create table` statement.
- StarRocks supports the loading of CSV files with multiple delimiters.

### Bug Fixes

The following bugs are fixed:

- Auto __op  mapping does not take effect if jsonpaths is specified in the command used for loading JSON data.
- BE nodes fail because the source data changes during data loading using Broker Load.
- Some SQL statements report errors after materialized views are created.
- The routine load does not work due to quoted jsonpaths.
- Query concurrency decreases sharply when the number of columns to query exceeds 200.

### Behavior Change

The API for disabling a Colocation Group is changed from `DELETE /api/colocate/group_stable` to `POST /api/colocate/group_unstable`.

### Others

flink-connector-starrocks is now available for Flink to read StarRocks data in batches. This improves data read efficiency compared to the JDBC connector.

## 2.1.2

Release date: March 14, 2022

### Bug Fixes

The following bugs are fixed:

- In a rolling upgrade from version 1.19 to 2.1, BE nodes stop working because of unmatched chunk sizes beween two versions. [#3834](https://github.com/StarRocks/starrocks/issues/3834)
- Loading tasks may fail while StarRocks is updating from version 2.0 to 2.1. [#3828](https://github.com/StarRocks/starrocks/issues/3828)
- Query fails when there is no appropriate execution plan for single-tablet table joins. [#3854](https://github.com/StarRocks/starrocks/issues/3854)
- A deadlock problem may occur when an FE node collects information to build a global dictionary for low-cardinality optimization. [#3839]( https://github.com/StarRocks/starrocks/issues/3839)
- Query fails when BE nodes are in suspended animation due to deadlock.
- BI tools cannot connect to StarRocks when  the show variables command fails.[#3708](https://github.com/StarRocks/starrocks/issues/3708)

## 2.1.3

Release date: March 19, 2022

### Bug Fixes

The following bugs are fixed:

- The problem of possible data loss due to BE failure (solved by using Batch publish version). [#3140](https://github.com/StarRocks/starrocks/issues/3140)
- Some queries may cause memory limit exceeded errors due to inappropriate execution plans.
- The checksum between replicas may be inconsistent in different compaction processes. [#3438](https://github.com/StarRocks/starrocks/issues/3438)
- Query may fail in some situation when JSON reorder projection is not processed correctly. [#4056](https://github.com/StarRocks/starrocks/pull/4056)

## 2.1.4

Release date: April 8, 2022

### New Feature

- The `UUID_NUMERIC` function is supported, which returns a LARGEINT value. Compared with `UUID` function, the performance of `UUID_NUMERIC` function can be improved by nearly 2 orders of magnitude.

### Bug Fixes

The following bugs are fixed:

- After deleting columns, adding new partitions, and cloning tablets,  the columns' unique ids in old and new tablets may not be the same, which may cause BE to stop working because the system uses a shared tablet schema. [#4514](https://github.com/StarRocks/starrocks/issues/4514)
- When data is loading to a StarRocks external table, if the configured FE of the target StarRocks cluster is not a Leader, it will cause the FE to stop working. [#4573](https://github.com/StarRocks/starrocks/issues/4573)
- The results of `CAST` function are different in StarRocks version 1.19 and 2.1. [#4701](https://github.com/StarRocks/starrocks/pull/4701)
- Query results may be incorrect, when a Duplicate Key table performs schema change and creates materialized view at the same time. [#4839](https://github.com/StarRocks/starrocks/issues/4839)
