# Kafka Partitioning Drill

**Recorded:** 2026-09-26

## Context

Kafka partitioning drill, Sep 26. The drill covered how to choose a partition count, hot partitions, and consumer scaling.

## Key points

Partition count is driven by throughput, not by item variety. The number of distinct keys does not set the partition count; the message rate and the processing rate per consumer do.

Hot partitions and consumer scaling were covered: a hot key can overload one partition while others sit idle, and consumers scale by adding consumers up to the number of partitions.

## Past mistakes & points to improve

- Justify partition count from throughput numbers, not from the number of distinct keys. Start from the message rate, divide by what one consumer can process, and let that set the partition count.
- When a hot partition is possible, say so explicitly and name the mitigation (a better key, or splitting the hot key) instead of assuming keys spread evenly.
