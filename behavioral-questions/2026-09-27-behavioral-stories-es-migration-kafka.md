# Behavioral Stories: Elasticsearch Migration and Kafka Pipeline

**Recorded:** 2026-09-27

## Elasticsearch 7 to 8 migration story

This story is told in STAR form, which means Situation, Task, Action, Result. It was graded A minus in a mock session.

The situation was a migration of Elasticsearch from version 7 to version 8 across 10 nodes, arranged as 5 primary and replica pairs. The task was to move the search index to the new version without losing data and without long downtime.

Two approaches were weighed. One was a rolling upgrade, node by node. The other was parallel dual-write, running the old and new versions side by side and writing to both. The migration used remote reindex and snapshot repository to move data between versions.

The products split into two groups. Five products were zero-loss Kafka products, each carrying about 20,000 messages per day, and these could not lose a single message. Ten products were reindexable from Hive or S3 (Simple Storage Service), so their data could be rebuilt from the source if needed.

A checksum script ran daily and produced a hit and miss report, so any data that failed to land in the new index was visible the same day. A disk-full incident in a lower environment was caught and handled during the work.

## eBay hiring manager practice

### Walk me through the Elasticsearch 7-to-8 migration. What made it risky, what did you own, and how did you verify the result?

We needed to migrate a 10-node production Elasticsearch cluster from version 7 to 8. It held critical customer data, and we had a narrow release window with zero tolerance for downtime or data loss.

I worked with our infrastructure team to evaluate a node-by-node rolling upgrade and a parallel migration. I proposed the parallel approach because it let us keep serving production reads from Elasticsearch 7 while we validated Elasticsearch 8. We added a parallel write job for the new cluster and migrated the historical indices separately.

I wrote reconciliation scripts to compare daily records by product across the two clusters. We tested the migration in lower environments for two to three weeks and ran the two write paths in production for a couple of weeks before switching reads. We completed the cutover with zero customer-facing downtime, and our production checks found no data loss.

### What did your reconciliation script compare, and how did you investigate mismatches?

For each product and day, I compared the number of new records indexed in Elasticsearch 7 and Elasticsearch 8. We also logged writes with the Kafka message ID and alerted on write failures in either cluster. We shipped the log files with Filebeat and searched them in Kibana. The counts showed where the clusters diverged, and the logs helped us identify failed messages.

Matching counts were an aggregate check. For a specific message flagged by the logs, we could look up its ID in both clusters.

### How did you recover a failed write, and what was the limit of that recovery path?

We used the failed message IDs to identify records that needed to be replayed and reindexed into the cluster that missed them. That was a manual recovery step. For these Kafka-backed products, we had no separate database to rebuild from, and Kafka retained messages for seven days. We had to act on alerts while the affected messages were still available.

### What did the disk-space incident in rehearsal teach you?

In a lower environment, both clusters shared a server. The disk filled up during dual writes, some writes failed, and our product counts diverged. Before production, we checked that there was enough disk headroom for the overlap and specified an alert to the infrastructure team at 80% disk utilization.

### How did you verify recovery after the disk-space incident?

Our recovery approach in the rehearsal was to rebuild the affected index. To verify recovery, we compared daily counts by product across both clusters and looked up the specific message IDs flagged as missing. The ID checks verified that those records were present in both clusters.

### How did you migrate historical data older than Kafka’s seven-day retention window?

We handled live ingestion and historical backfill separately. The existing job continued writing to Elasticsearch 7, and we added a parallel job for Elasticsearch 8. For the historical data already in Elasticsearch 7, I identified the cutoff and used a snapshot of the daily indices to migrate that data to Elasticsearch 8.

### How did you choose the cutoff and cover the full cutoff day?

We created one index per day. We deployed the Elasticsearch 8 writer during the weekend release window so it was ready before trading opened on Monday. The snapshot included all historical daily indices through Friday. Monday was the first day covered by the Elasticsearch 8 live writer.

### What happened to Saturday and Sunday?

These products only collected data during trading hours, so there was no weekend data to backfill.

### What was your rollback plan after switching production reads?

If we discovered missing or incorrect results, we could point production reads back to Elasticsearch 7. We kept it online and continued writing to it so it stayed current and could serve as a live fallback.

### When did you stop writing to Elasticsearch 7, and why keep both write paths for six months?

We retained six months of trading data. Once Elasticsearch 8 had received a full six months of live data, its live ingestion covered the entire retention window. We stopped writing to Elasticsearch 7 at that point so we could reclaim capacity.

Keeping Elasticsearch 7 current gave us a live fallback while we observed the new cluster under production traffic. We accepted the extra write and storage cost for one retention cycle so we could switch reads back if we discovered a configuration or network issue.

### What evidence would make you comfortable retiring Elasticsearch 7 sooner?

Six months was our most conservative choice because by then Elasticsearch 8 had received the entire retention window through live ingestion. If I did it again, I’d agree on earlier exit criteria with infrastructure. I’d reconcile critical Kafka-backed products by message ID, confirm there were no unresolved write failures, and verify that search latency, error rates, and disk headroom stayed within agreed limits for a defined period under production traffic. Once we met those criteria, I’d be comfortable stopping writes to Elasticsearch 7 sooner.

### Open follow-up on duplicate prevention

**Question:** If a replay included records that were already indexed, how would you prevent duplicate documents?

**To confirm before answering:** Which Elasticsearch document ID and indexing operation did the replay use?

## Kafka pipeline story

The pipeline is Spring Boot Kafka consumers that index into Elasticsearch, plus building search queries from user input. Messages went to a 3-partition topic with 7-day retention, then were gone forever. Per-product jobs handled each product separately, and Oracle, S3, and Hive scheduled jobs fed the pipeline.

## Honesty incident

On the same day, Robin nearly re-presented a database (DB) failover design as finished work. The design was 3 retries, then a database write plus an email alert. It had not been built yet. He caught himself before presenting it as done, and the assistant refused to help dress it up as finished work.

## Past mistakes & points to improve

- The near-miss with the database failover story is the rule, not the exception. Every resume line is a promise of interview depth, so mark built work and planned work explicitly in every story. If a design was planned but never built, say so in the first sentence.
- The Elasticsearch story is graded A minus, which is strong, but the grade depends on the details being true. Keep the numbers attached to the story (10 nodes, 5 primary and replica pairs, 5 zero-loss Kafka products, 10 reindexable products) and do not round them up or embellish them under pressure.

## Source

Prepared by Copper bot from Robin's interview practice sessions, September–November 2026. Ask Copper bot for more detail on anything in this note.

The Elasticsearch migration Q&A comes from Robin’s eBay hiring manager interview practice in November 2026. Codex organized the follow-up questions and polished the answers. Ask Codex for more detail about this practice session.
