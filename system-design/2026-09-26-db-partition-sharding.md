# Database Partition and Sharding Crash Course

**Recorded:** 2026-09-26

## Context

Database partition and sharding crash course, Sep 26. The course covered how a partition key is chosen in different databases and how time-series data is rolled up.

## Key points

Oracle versus Cassandra partition key: the partition key decides which node holds the data, and the choice differs between the two systems in how it is declared and used.

TTL (time to live) controls how long a row lives before it expires, which is how old time-series data is dropped without a manual delete.

Rollup granularity runs minute to hour to day: raw events are aggregated into minute buckets, then hour buckets, then day buckets, so older windows cost less to query.

Shard count 128 was used as the working number in the course.

## Past mistakes & points to improve

- State the rollup granularity ladder explicitly when designing time-series storage. Say minute to hour to day out loud, with what each level stores, instead of leaving the aggregation levels vague.
- Name the TTL (time to live) policy for old data in the same pass. Expiry is part of the storage design, not an afterthought.

## Source

Prepared by Copper bot from Robin's interview practice sessions, September–November 2026. Ask Copper bot for more detail on anything in this note.
