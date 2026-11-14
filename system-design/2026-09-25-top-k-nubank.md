# Top-K (Nubank Demon)

**Recorded:** 2026-09-26

## Context

Mock 2, Day 1 mock, Sep 25-26. Top-K, known as the Nubank demon. The problem is to serve top-K queries over billions of events.

## First pass

The first pass was graded D on correctness. The design was: UI (user interface) to API (application programming interface) to a query against the sale-history database on every request, at billions of events. A per-request database query cannot serve that volume, so the design was wrong at the scale that matters.

## Redeemed design

The redeemed design uses write-path pre-aggregation. Events flow through Kafka to a stream processor, which aggregates them into per-minute buckets in a serving store. The read path then composes arbitrary time windows from those buckets and picks the top K with a heap.

One smell was noted in the redeemed design: a shared cache is unnecessary shared state. Partition by item_id instead of sharing state across the serving path.

## Grade

First pass grade D on correctness. The redemption came from moving the aggregation to the write path instead of doing it on every read.

## Past mistakes & points to improve

- Find the hard scale path first. Before drawing anything, ask where the volume actually lands: reads or writes, and at what rate.
- Never answer a read-heavy aggregation question with a per-request database query. If the query touches billions of events, the aggregation must happen on the write path and the read must serve pre-computed results.
- Prefer per-minute buckets plus a heap for arbitrary windows over a shared cache. Partition by the item key instead of introducing shared state that every reader must coordinate through.

## Source

Prepared by Copper bot from Robin's interview practice sessions, September–November 2026. Ask Copper bot for more detail on anything in this note.
