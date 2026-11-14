# Kafka Event-Driven Pipeline

**Recorded:** 2026-09-27

## The real scope

The real production work is Spring Boot Kafka consumers that index into Elasticsearch, plus building search queries from user input. That is the whole of the lived experience. Everything else about Kafka internals is learned, not lived.

## What was built

Messages went to a 3-partition topic with 7-day retention, then were gone forever. Per-product jobs handled each product separately. Oracle, S3 (Simple Storage Service), and Hive scheduled jobs fed the pipeline.

The concepts Robin can speak to from the real pipeline are partitions, idempotency, and time-window aggregation. Those are the anchors for every Kafka answer.

## Honesty boundary

Robin has NOT operated Kafka or Cassandra directly. The internals of Kafka and Cassandra are learned concepts, not lived experience. Presenting them as production experience would be dishonest, and it would not survive follow-up questions from an interviewer who has operated them.

## Past mistakes & points to improve

- Keep every Kafka answer anchored to the real pipeline: partitions, idempotency, and time-window aggregation. Start from what was actually built, then say clearly where the knowledge is learned rather than lived.
- Bluffing learned internals as experience is the fastest way to fail the Architecture round. When a question goes past the real pipeline, say so plainly and explain the concept as a concept, not as something that was operated in production.

## Source

Prepared by Copper bot from Robin's interview practice sessions, September–November 2026. Ask Copper bot for more detail on anything in this note.
