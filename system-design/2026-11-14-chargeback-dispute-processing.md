# Chargeback / Dispute Pipeline — Async and Rate-Limited Processing

**Recorded:** 2026-11-14  
**Practice period:** Late August–September 2026

## Problem

Accept financial disputes safely, persist a canonical lifecycle, and coordinate with a downstream card-network/partner API whose capacity may be lower than incoming demand (example discussed: **50 requests per second**).

## Design

1. **Create dispute API:** authenticate, validate, accept a client idempotency key, atomically persist dispute with state `PENDING` and an outbox record.
2. **Outbox publisher:** reliably emits `DisputeCreated` to Kafka; do not assume DB write and Kafka publish are one transaction.
3. **Rate-controlled workers:** consumer group processes events; token bucket / central limiter protects downstream 50-rps quota across all replicas.
4. **State machine:** `PENDING → SUBMITTING → SUBMITTED / RETRYABLE_FAILURE / FAILED`, including timestamps, attempts, provider IDs and deadlines.
5. **External callbacks/reconciliation:** match partner status to canonical dispute ID; dedupe callbacks and handle out-of-order updates.

## Failure cases to narrate

- **Client retries POST:** same idempotency key + payload produces same operation/result, not a new dispute. Define retention and conflict semantics.
- **Worker crashes after partner success before offset commit:** message repeats. Use provider-side idempotency key where supported, query status, or reconcile; Kafka offset alone cannot undo external side effects.
- **Partner throttles or fails:** global rate limiter, bounded exponential backoff with jitter, circuit breaker and deadline-aware retries.
- **Poison message:** DLQ with context/alert and safe replay after correction; do not silently lose financial actions.
- **Kafka unavailable after DB write:** outbox survives until publisher resumes.
- **Out-of-order events:** enforce valid state transitions using version/CAS or transactional logic.

## Capacity and load

If arrival rate exceeds 50 rps sustainably, queue depth grows. Rough first-order growth: `(incoming_rps − effective_processing_rps) × seconds`. Adding workers helps only until the partner limit; choose backpressure, queue-time alerts or admission limits.

## Past mistakes and feedback

- **Observed coaching:** You understood Kafka and idempotency well in isolation, but earlier system design benefited from explicitly mapping each concept to a failure boundary.
- **Potential trap:** Calling Kafka “exactly once” does not guarantee one external card-network submission.
- **Potential trap:** Increasing consumer count does not solve a fixed partner quota.

## Points to improve

1. Draw command/write path and async side effects separately.
2. Walk through **DB commit → publish → worker → partner response → offset commit**, inserting a crash between every pair.
3. Define idempotency key uniqueness, TTL, payload conflicts, reconciliation and DLQ replay.
4. Estimate worst-case backlog and deadline misses under spikes.

## Source

- **Documentation assistant:** ChatGPT (OpenAI).
- **Source material:** Robin's prior ChatGPT interview-preparation and AI-learning discussions, consolidated into this study note; not a verbatim transcript.
- **Discussion reference:** Practice period: Late August–September 2026.
- **For follow-up:** Share [Chargeback / Dispute Pipeline — Async and Rate-Limited Processing](https://github.com/szlike/personal-vault/blob/main/system-design/2026-11-14-chargeback-dispute-processing.md) with ChatGPT and ask for a deeper explanation, practice question, or review of past mistakes. Original chat permalinks were not recorded.
