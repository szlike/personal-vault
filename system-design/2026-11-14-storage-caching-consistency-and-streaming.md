# Data Stores, Caching, Consistency, and Real-Time Pipelines

**Recorded:** 2026-11-14  
**Practice period:** September–November 2026

## Choosing components by access pattern

| Need | Typical choice | Trade-off to name |
| --- | --- | --- |
| Financial command/state | Transactional SQL DB | Consistency and constraints vs scale of central writes |
| Event replay / async fan-out | Kafka | Partitions, lag, retention, operations |
| Search and aggregations | Elasticsearch | Index freshness, mappings, expensive queries, reindex |
| Hot read optimization | Redis/cache | Invalidation, staleness, eviction and memory |
| Large files / ETL | Object storage + job metadata | Retry granularity, partial failure, data lifecycle |
| Real-time client updates | WebSocket + snapshot API | Disconnect/replay, fan-out, eventual client state |

## Consistency/availability

- Decide whether an operation must be strongly consistent: accepting a bid, reserving one unique item, submitting a dispute.
- Read models/search can be eventually consistent if the product permits staleness and displays authoritative confirmation elsewhere.
- Replication improves durability/read capacity but creates failover, lag and leader-selection questions.
- Sharding scales data and throughput but changes transactions, cross-shard queries and hotspots.

## Real-time flow

Ingestion → durable processing → read model → WebSocket push. Maintain server-side authoritative state, client reconnect strategy, snapshots and sequence numbers where order matters. Use batching/coalescing for high-frequency chart updates without silently dropping important domain events.

## Past mistakes and feedback

- **Observed:** Strong Elasticsearch/visualization hands-on experience, but interview answers should also explain failure boundaries, consistency and why a simpler database alone is insufficient.
- **Coaching gap:** Database isolation, sharding and cloud/reliability depth need deliberate practice for Staff-level architecture discussions.
- **Potential error:** Treating a cache/search index as authoritative for inventory or accepted financial actions.

## Points to improve

1. For each project, state source of truth, query/read models and acceptable staleness.
2. Compare SQL vs Elasticsearch on one real query and the indexing consistency requirement.
3. Explain failover, retries and replay during a Kafka consumer/Elasticsearch outage.
4. Design WebSocket reconnect from last seen event ID or a full fresh snapshot.

## Source

- **Documentation assistant:** ChatGPT (OpenAI).
- **Source material:** Robin's prior ChatGPT interview-preparation and AI-learning discussions, consolidated into this study note; not a verbatim transcript.
- **Discussion reference:** Practice period: September–November 2026.
- **For follow-up:** Share [Data Stores, Caching, Consistency, and Real-Time Pipelines](https://github.com/szlike/personal-vault/blob/main/system-design/2026-11-14-storage-caching-consistency-and-streaming.md) with ChatGPT and ask for a deeper explanation, practice question, or review of past mistakes. Original chat permalinks were not recorded.
