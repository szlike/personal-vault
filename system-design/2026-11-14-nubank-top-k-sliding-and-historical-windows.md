# Nubank Retrospective — Top-K Items Over Time Windows

**Recorded:** 2026-11-14  
**Original design discussion:** 2026-09-04

## Prompt (as recalled)

Track the most visited/accessed **K items per second** over the last hour and support querying any historical hour. Exact interpretation of 'per second' and whether windows may start at arbitrary seconds should be clarified.

## Minimal architecture

**Clients → ingestion API → Kafka partitioned by item ID → stream aggregators → durable bucketed counts → Top-K query/serving API.**

Maintain per-item counts at a suitable bucket resolution (e.g. seconds for second-level accuracy). For last-hour query, sum the previous 3,600 one-second buckets for relevant items or use an incrementally maintained rolling count. For historical hours, persist compact buckets and compute/materialize rankings. If queries are fixed aligned hours, hourly results can be precomputed; arbitrary 60-minute historical windows require finer-grained buckets or a more complex index.

## Algorithms and trade-offs

- **Aggregation:** local counts keyed by `(bucket_timestamp, item_id)`; combine partitioned results if processing is distributed.
- **Top-K:** min-heap of size K on final counts (`O(U log K)` over U distinct items). Do **not** assume a heap stays valid when rolling counts decrease without recalculation/lazy invalidation.
- **Exact vs approximate:** exact counters can be expensive at huge cardinality; sketch/heavy-hitter algorithms trade accuracy for state/throughput.
- **State:** processor checkpoint + changelog, or durable bucket store; handle process crash and replay.
- **Time:** event time vs processing time, late events, allowed lateness and bucket correction policy.
- **Queries:** cache frequent windows but invalidate or version if late events change results.

## Scale / fault walkthrough

1. Clarify events/sec, distinct items, retention, result freshness, K and historical-query SLA.
2. Acknowledge ingestion only after durable acceptance; assign stable event IDs if deduplication is needed.
3. Partition events for key-local aggregation; explain repartitioning or merging results for global Top-K.
4. Save checkpoints and counts, monitor lag/late events and support reprocessing.
5. Return a snapshot time and clarify whether results are eventually consistent or exact through a watermark.

## Past mistakes and feedback

- **Confirmed interview setback:** You explored details/requirements for too long and did not finish the high-level design.
- **Important correction:** Knowing Kafka, partitions and min-heaps is not enough; a Top-K query cannot be served correctly without specifying the window, counts, completeness and aggregation path.
- **Potential conceptual trap:** “Store top K per minute and combine” may miss items that never made a local top K but rank globally after aggregation. Exactness needs sufficient candidate coverage or complete aggregated counts.

## Points to improve

1. Draw the 6-box architecture in under five minutes and choose exact vs approximate explicitly.
2. Explain aligned-hour vs arbitrary-hour queries with an example.
3. Derive storage: `buckets × active_items_per_bucket × bytes_per_counter` plus metadata.
4. Practice late events, duplicate delivery, hot keys, repartitioning and recovery.
5. Do a 45-minute redo of the same prompt and time each phase.

