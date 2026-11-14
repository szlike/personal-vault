# Kafka — Partitions, Consumers, Delivery, and Transactional Outbox

**Recorded:** 2026-11-14  

## Source

**Prepared by:** Codex (OpenAI)

**Basis:** Consolidated interview-preparation material in this repository. Treat examples, metrics, and ownership as prompts to verify against your firsthand experience; return to Codex with the note title for expansion or correction.
**Practice period:** September–November 2026

## Core concepts

- **Topic/partition:** ordered append-only log *within a partition*. A key chooses a partition; no general ordering guarantee across partitions.
- **Consumer group:** partitions assigned among consumers; for one group a partition is actively processed by one consumer at a time (rebalances complicate ownership transitions).
- **Offset:** progress position. Commit timing determines replay/possible loss behavior.
- **Replication/acks:** producer durability depends on replication, ISR, `acks`, retries and producer settings; `acks=all` should be understood with `min.insync.replicas`.
- **Worker vs consumer:** worker is a general unit doing work; a Kafka consumer is a client reading topic partitions.

## Delivery semantics

- **At-most-once:** commit progress before processing; a crash can lose work.
- **At-least-once:** process then commit; a crash after side effect but before commit can replay.
- **Exactly-once:** possible in scoped Kafka transactional pipelines, but **not automatically across an arbitrary SQL DB, Elasticsearch index or external API**.

## How to obtain effective exactly-once business effect

1. Give each business command/event a stable unique ID.
2. Enforce idempotency in the destination, e.g. unique key + transaction or deterministic upsert.
3. Commit consumer offset after the durable effect; expect redelivery on failure.
4. Use transactional outbox for atomic DB update + intent to publish, then publisher retries.
5. Reconcile external systems when their effect and local record cannot be one transaction.

## Operational deep dive

Partition-count vs consumer-count trade-off; ordering by entity; partition skew/hot keys; replay/reprocessing; schema evolution; consumer lag; rebalance; poison messages; DLQs; retention vs durable domain data.

## Past mistakes and feedback

- **Observed strength:** Kafka/async/idempotency explanations were among your stronger areas during Nubank preparation.
- **Observed improvement need:** Explicitly connect offset timing, transaction boundary and downstream state instead of giving a generic list of Kafka benefits.
- **Potential misconception to avoid:** A topic with more partitions does not automatically preserve global event order.

## Points to improve

1. Explain one message's full lifetime with three possible crash points.
2. Compare choosing entity ID vs random partition key and consequences for ordering/hot partitions.
3. State what application-level idempotency guarantees, then what Kafka itself guarantees.
4. Practice describing `acks=all`, replication factor, ISR and producer retries without treating them as synonyms.

## Source

- **Documentation assistant:** ChatGPT (OpenAI).
- **Source material:** Robin's prior ChatGPT interview-preparation and AI-learning discussions, consolidated into this study note; not a verbatim transcript.
- **Discussion reference:** Practice period: September–November 2026.
- **For follow-up:** Share [Kafka — Partitions, Consumers, Delivery, and Transactional Outbox](https://github.com/szlike/personal-vault/blob/main/system-design/2026-11-14-kafka-consumers-delivery-and-outbox.md) with ChatGPT and ask for a deeper explanation, practice question, or review of past mistakes. Original chat permalinks were not recorded.
