# Goldin Marketplace System-Design Themes

**Recorded:** 2026-11-13

## Relevant domains

- Real-time auction bidding, anti-sniping behavior, and audit trails
- Unique-item inventory and order lifecycle
- Authentication, grading, provenance, and chain of custody
- Search and discovery across collectibles and historical sales
- Payments, seller payouts, disputes, fraud, and risk controls

## Design principles

Prioritize correctness, traceability, idempotency, resilience, and clear customer communication. In a high-value marketplace, trust is a core product feature.

## Project-story template

1. Customer or business problem
2. Your role, scope, and constraints
3. Technical design and trade-offs
4. Leadership and cross-team influence
5. Outcome, metrics, and lessons

## Past mistakes and lessons

- **Treating a high-value marketplace like a standard catalog.** Unique items require an authoritative lifecycle, reservation rules, and a clear answer to “who owns the current state?” Caches and search indexes cannot be the source of truth for ownership or purchase eligibility.
- **Optimizing only the happy path for bidding.** Concurrent bids, retries, clock boundaries, anti-sniping extensions, and delayed client updates are core correctness cases, not edge details.
- **Using “exactly once” as an implementation claim.** Network retries and downstream failures mean the practical design needs idempotency keys, durable state transitions, deduplication, and observable reconciliation.
- **Leaving auditability as a logging afterthought.** For valuable collectibles, append-only event history, actor identity, timestamps, state transitions, and retention/access controls should be deliberate product and compliance requirements.
- **Ignoring the human recovery path.** Fraud reviews, disputes, grading exceptions, and payment failures need controlled manual workflows, permissions, and customer communication—not just automatic state machines.
- **Adding components without a failure model.** A queue, cache, or event stream introduces ordering, replay, latency, and availability trade-offs. State how the system behaves when each dependency is slow, unavailable, or duplicates a message.
- **eBay seller dashboard round, Sep 29 (Usha Achladi):** First pass graded C+. No BFF (backend for frontend), no write path, mathed the 0.6 rps read path and missed the 5B-evaluation scheduled job. Fix: the scheduled-job / write path is the hard scale path in dashboard problems — find it before doing read-path arithmetic. Specify the BFF, state every choice as a trade-off, and name a failure mode per component.
- **Notifications mock (Depop), Sep 25:** When asked where dedup happens, he answered "what dedup?" — he could not explain the concept unprompted despite idempotency being listed among his Kafka strengths. Fix: state the dedup mechanism unprompted. Kafka is at-least-once delivery; the worker enforces effectively-once by checking a unique event ID against the notification store before pushing, with check-and-insert atomic via unique constraint.
- **Top-K mock (Nubank), Sep 25-26:** First pass graded D on correctness — UI to API to query the sale-history DB per request at billions of events. Fix: find the hard scale path first. Never answer a read-heavy aggregation question with a per-request DB query; pre-aggregate on the write path (per-minute buckets, Kafka to stream processor to serving store) and compose arbitrary windows on the read path.

## Improvements and practice

1. Practice an auction design around a single authoritative bid acceptance path, including ordering, idempotency, anti-sniping, bidder notifications, and an immutable audit record.
2. Draw the item lifecycle from intake and authentication through listing, bid/reservation, payment, fulfillment, dispute, and payout; define legal state transitions and owners.
3. For each asynchronous workflow, name the idempotency key, durable record, retry policy, dead-letter/reconciliation process, and user-visible status.
4. Make trust measurable: bid acceptance latency, duplicate-action rate, reconciliation lag, dispute rate, provenance-completeness rate, and time to restore an incorrect state.
5. Rehearse trade-offs between strong consistency for scarce inventory and eventual consistency for search, analytics, and notifications.

## Source

The session-specific bullets in "Past mistakes and lessons" were added by Copper bot from Robin's September–October 2026 practice sessions. Ask Copper bot for more detail on any of them.
