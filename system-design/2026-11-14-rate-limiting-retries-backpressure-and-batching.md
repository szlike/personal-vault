# Rate Limiting, Retries, Backpressure, and Batch Processing

**Recorded:** 2026-11-14  
**Practice period:** September–November 2026

## Rate limiting

- **Ingress rate limit:** protects your API; fixed/sliding window or token bucket at gateway/service.
- **Downstream rate limit:** protects a partner quota even if ingestion can accept more work. Use a shared/distributed limiter or coordinated worker budget.
- **Token bucket:** tokens refill at rate R up to burst B. Check fairness and multi-replica atomicity.
- **429 handling:** respect `Retry-After`, jitter and cap attempts; do not immediately requeue in a tight loop.

## Queue backpressure

Monitor arrival vs service rates. If producer throughput > sustained consumer capacity, queue grows. Options: autoscale to available capacity, limit producer, batch/coalesce where semantically safe, prioritize/deadline, and apply load shedding only when dropping work is permissible.

## Retries and DLQ

- Transient network/5xx → bounded exponential backoff, jitter and circuit breaker.
- Permanent validation/4xx → reject or DLQ; avoid infinite retries.
- For financial commands, idempotency protects each attempt and reconciliation handles uncertain outcomes.
- DLQ must include original event ID, error, attempt count and replay strategy; set operational alerts.

## Batch / ETL prompt

Earlier practice considered approximately **10,000 records / 10 files** and asynchronous handling. Clarify whether 10K means records per file or total, batch latency/SLA, ordering, permissible partial success and memory constraints.

Proposed design: object upload → metadata job record → per-file work items → bounded parallel workers → validation/transform → idempotent writes → per-file progress/result summary. Choose streaming parsing over loading large files fully; checkpoint and retry at file/chunk boundaries.

## Past mistakes and feedback

- **Observed discussion point:** Worker count and consumer count are not necessarily equivalent, and neither bypasses a 50-rps downstream limit.
- **Frequent interview pitfall:** Treating retries as harmless without idempotency and deadlines.
- **Missing context:** The exact production batch and service quotas were interview exercises, not established Citi production metrics.

## Points to improve

1. Quantify backlog and drain time for 100 rps in, 50 rps out, and a 10-minute burst.
2. Compare fixed window vs token bucket with a concrete burst scenario.
3. Explain partial batch failure, replay granularity, poison record isolation and progress tracking.
4. Practice how to maintain order while increasing consumer concurrency.

## Source

- **Documentation assistant:** ChatGPT (OpenAI).
- **Source material:** Robin's prior ChatGPT interview-preparation and AI-learning discussions, consolidated into this study note; not a verbatim transcript.
- **Discussion reference:** Practice period: September–November 2026.
- **For follow-up:** Share [Rate Limiting, Retries, Backpressure, and Batch Processing](https://github.com/szlike/personal-vault/blob/main/system-design/2026-11-14-rate-limiting-retries-backpressure-and-batching.md) with ChatGPT and ask for a deeper explanation, practice question, or review of past mistakes. Original chat permalinks were not recorded.
