# Behavioral Stories: Elasticsearch Migration and Kafka Pipeline

**Recorded:** 2026-09-27

## Elasticsearch 7 to 8 migration story

This story is told in STAR form, which means Situation, Task, Action, Result. It was graded A minus in a mock session.

The situation was a migration of Elasticsearch from version 7 to version 8 across 10 nodes, arranged as 5 primary and replica pairs. The task was to move the search index to the new version without losing data and without long downtime.

Two approaches were weighed. One was a rolling upgrade, node by node. The other was parallel dual-write, running the old and new versions side by side and writing to both. The migration used remote reindex to move data between versions.

The products split into two groups. Five products were zero-loss Kafka products, each carrying about 20,000 messages per day, and these could not lose a single message. Ten products were reindexable from Hive or S3 (Simple Storage Service), so their data could be rebuilt from the source if needed.

A checksum script ran daily and produced a hit and miss report, so any data that failed to land in the new index was visible the same day. A disk-full incident in a lower environment was caught and handled during the work.

## Kafka pipeline story

The pipeline is Spring Boot Kafka consumers that index into Elasticsearch, plus building search queries from user input. Messages went to a 3-partition topic with 7-day retention, then were gone forever. Per-product jobs handled each product separately, and Oracle, S3, and Hive scheduled jobs fed the pipeline.

## Honesty incident

On the same day, Robin nearly re-presented a database (DB) failover design as finished work. The design was 3 retries, then a database write plus an email alert. It had not been built yet. He caught himself before presenting it as done, and the assistant refused to help dress it up as finished work.

## Past mistakes & points to improve

- The near-miss with the database failover story is the rule, not the exception. Every resume line is a promise of interview depth, so mark built work and planned work explicitly in every story. If a design was planned but never built, say so in the first sentence.
- The Elasticsearch story is graded A minus, which is strong, but the grade depends on the details being true. Keep the numbers attached to the story (10 nodes, 5 primary and replica pairs, 5 zero-loss Kafka products, 10 reindexable products) and do not round them up or embellish them under pressure.

## Source

Prepared by Copper bot from Robin's interview practice sessions, September–November 2026. Ask Copper bot for more detail on anything in this note.
