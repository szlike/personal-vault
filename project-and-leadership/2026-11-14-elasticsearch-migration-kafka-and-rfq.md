# Elasticsearch Migration, Kafka Pipelines, and RFQ Ownership

**Recorded:** 2026-11-14  
**Practice period:** September–November 2026

## Elasticsearch 7 → 8 migration

**Known experience:** Work on migration to Elasticsearch 8, involving a new cluster and remote reindex strategy. Interview-worthy issues: mappings and analyzers, client/query compatibility, indices/aliases, permissions, large multi-product data, validation and cutover.

**Answer sequence:** requirement/constraint → old/new topology → why remote reindex over alternatives → traffic/data consistency during copying → validation and recovery → outcome. Only claim particular dual-write, alias, rollback or automation steps if actually performed.

**Technical questions to rehearse:** What happens to writes during reindex? How do you check duplicates/missing documents? How are index templates and analyzers made compatible? What are the rollback criteria? Can Kafka replay cover a missing window?

## Kafka / Elasticsearch data ingestion

**Known experience:** Kafka-backed event processing and Elasticsearch search/aggregation for trading data. A useful discussion flow is producer → topic/partition key → consumer group → transformation → idempotent index write → monitoring/query API.

**Deep dive:** Partition-key ordering; consumer offset commit after durable side effects; redelivery/upserts; reprocessing on schema changes; batching, retries, poison records and lag monitoring. The exact semantics of the real platform need confirmation.

## RFQ product and technical ownership

RFQ = request for quote. Prepare a real example showing requirement ambiguity, frontend/backend integration, product prioritization, correctness, customer/stakeholder needs, and measurable delivery. Distinguish Product Owner prioritization decisions from engineering architecture decisions.

## Past mistakes and feedback

- **Observed:** Going straight to Kafka, reindexing or frontend details before explaining the overall objective and risk.
- **Gap:** We do not have verified project-specific throughput time units, data loss figures, exact timeline or quantified outcome.
- **Interview trap:** “Exactly once” is not a free Kafka guarantee for arbitrary database/index writes; explain idempotency or transactional boundaries.

## Points to improve

1. Create an architecture diagram with actors, data stores, event ordering and failure boundaries.
2. Write actual migration numbers, validation plan, a setback and rollback criteria; mark unknowns until verified.
3. Explain one real technical trade-off and how you obtained agreement.
4. Prepare the question “Which part did **you** lead?” with a precise answer.

## Source

- **Documentation assistant:** ChatGPT (OpenAI).
- **Source material:** Robin's prior ChatGPT interview-preparation and AI-learning discussions, consolidated into this study note; not a verbatim transcript.
- **Discussion reference:** Practice period: September–November 2026.
- **For follow-up:** Share [Elasticsearch Migration, Kafka Pipelines, and RFQ Ownership](https://github.com/szlike/personal-vault/blob/main/project-and-leadership/2026-11-14-elasticsearch-migration-kafka-and-rfq.md) with ChatGPT and ask for a deeper explanation, practice question, or review of past mistakes. Original chat permalinks were not recorded.
