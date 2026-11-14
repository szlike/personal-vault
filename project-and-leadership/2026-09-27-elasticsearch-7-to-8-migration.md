# Elasticsearch 7 to 8 Migration

**Recorded:** 2026-09-27

## The project

This is the flagship story from Robin's work at Citi (Citibank): migrating Elasticsearch from version 7 to version 8. It is told in STAR form, which means Situation, Task, Action, Result, and it was graded A minus in a mock session.

## Situation and task

The search index ran on 10 Elasticsearch nodes, arranged as 5 primary and replica pairs. The task was to move the index to version 8 without losing data and without long downtime.

## Action

Two approaches were weighed. One was a rolling upgrade, node by node. The other was parallel dual-write, running the old and new versions side by side and writing to both. The migration used remote reindex to move data between versions.

The products split into two groups. Five products were zero-loss Kafka products, each carrying about 20,000 messages per day, and these could not lose a single message. Ten products were reindexable from Hive or S3 (Simple Storage Service), so their data could be rebuilt from the source if needed.

A checksum script ran daily and produced a hit and miss report, so any data that failed to land in the new index was visible the same day. A disk-full incident in a lower environment was caught and handled during the work.

## Result

The migration completed with the data intact. The checksum report gave daily proof that the new index matched the old one, and the zero-loss products lost nothing.

## Past mistakes & points to improve

- The resume bullet wording for this migration is still an open item. The story itself is solid and graded A minus, but the bullet on the resume has not been confirmed. Confirm the wording before the next application so the resume and the story match exactly.

## Source

Prepared by Copper bot from Robin's interview practice sessions, September–November 2026. Ask Copper bot for more detail on anything in this note.
