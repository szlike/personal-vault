# System Design Interview Framework — Diagram First

**Recorded:** 2026-11-14  
**Practice period:** September–November 2026

## Core response structure

1. **Requirements (3–5 min):** users, primary operations, scale, latency, consistency, durability, scope exclusions.
2. **Architecture (by minute 5–10):** draw clients/API, core services, source-of-truth storage, queue/event bus, worker/stream processors and read path.
3. **API/data model:** name endpoints, entities, keys and state transitions.
4. **Scale:** estimate volume and partition/index/cache decisions, only where relevant.
5. **Failure:** duplicates, retries, concurrency, DLQ, lag, replay, deployment/rollback.
6. **Deep dive/trade-offs:** go deeper when interviewer asks; recap consistency and bottlenecks.

## Example opening

> Let me confirm the most important requirement: are we optimizing for exact results and historical queries, or lower-latency approximate results? I'll outline the end-to-end architecture first, then discuss partitioning and failure handling.

## Concrete reminders

- Distinguish **source of truth** (DB/log) from derived cache/index/read model.
- Specify **where** idempotency and ordering are enforced, not only that “Kafka supports them.”
- Derive scale numbers from explicit assumptions; identify the dominant bottleneck.
- If limited by an external 50-rps API, adding more workers does not increase the allowed downstream rate.
- Start with a straightforward workable design. Name evolution paths for higher traffic.

## Past mistakes and feedback

- **Directly observed/reported:** In the Nubank Top-K interview, time was exhausted before a complete design was presented. Requirements/implementation detail came before architecture.
- **Repeated feedback:** You know many Kafka/idempotency concepts but need faster **pattern recognition** and an answer sequence under pressure.
- **Communication trap:** Long lists of technologies without the dataflow, constraints or trade-offs read as scattered knowledge.

## Points to improve

1. Set a hard target: full architecture on board by minute 8.
2. Practice three deliberately unfamiliar prompts; reserve the last five minutes for failure discussion.
3. Use “short answer → mechanism → trade-off → stop” for follow-ups.
4. Draw arrows for commands and data separately; state consistency per operation.
5. Review one bad design answer per week and rewrite it as a one-page design doc.

## Source

- **Documentation assistant:** ChatGPT (OpenAI).
- **Source material:** Robin's prior ChatGPT interview-preparation and AI-learning discussions, consolidated into this study note; not a verbatim transcript.
- **Discussion reference:** Practice period: September–November 2026.
- **For follow-up:** Share [System Design Interview Framework — Diagram First](https://github.com/szlike/personal-vault/blob/main/system-design/2026-11-14-system-design-interview-framework.md) with ChatGPT and ask for a deeper explanation, practice question, or review of past mistakes. Original chat permalinks were not recorded.
