# Notification System for a Marketplace (Depop, 50M Users)

**Recorded:** 2026-09-25

## Context

Mock 1, Day 1 mock, Sep 25. Design a near real-time notification system for a marketplace like Depop, with 50 million users. Sellers must be notified within seconds on web and mobile when a buyer acts.

## First response

The first response was limited to requirements, scale estimates, and high-level architecture. The lesson from this mock is diagram first, details second: draw the system before going deep on any one part.

## Architecture

The iterated architecture is: buyer-actions API (application programming interface), then a service, then Kafka, then a worker, then push delivery to web and mobile.

The redrawn flow adds three stores: a notification event database, a user-preference database, and a dedup status update. The user-preference database decides whether a given user wants a given notification. The notification event database records what was sent.

## Deduplication

When asked where dedup happens, the answer in the mock was "what dedup?" The concept could not be explained unprompted, even though idempotency was listed among his Kafka strengths. That gap is a resume-depth risk: a claimed strength must be explainable on demand.

The pattern taught on the spot: Kafka gives at-least-once delivery, so the same event can arrive more than once. The worker enforces effectively-once delivery by checking a unique event ID against the notification store before pushing, with the check-and-insert made atomic through a unique constraint.

Outbox plus atomic dedup was established as the combined mechanism: a single atomic insert, with a primary-key conflict acting as the dedup signal. One hole was named explicitly: a crash between the insert and the push leaves an event recorded but never delivered.

## Grade

Grade B-, rough start and strong finish. The finish was strong because the architecture was iterated and the dedup pattern was learned and redrawn, but the start was rough because the first pass stayed at requirements and estimates instead of drawing the system.

## Past mistakes & points to improve

- State the dedup mechanism unprompted in any event-driven design. If the design uses Kafka, say in the first pass that delivery is at-least-once and that the worker enforces effectively-once with a unique event ID and an atomic check-and-insert.
- Diagram first, details second. Draw the full flow (API, service, Kafka, worker, push, and the stores) before spending time on scale estimates for any single part.
- A claimed strength must be explainable cold. Idempotency was on the resume as a Kafka strength but could not be explained when asked, so rehearse a one-sentence definition plus the mechanism for every claimed strength.
- Name the crash hole when presenting an outbox or dedup design. The crash between insert and push is a real failure mode, and naming it shows the design was thought through.
