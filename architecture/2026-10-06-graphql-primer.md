# GraphQL Primer

**Recorded:** 2026-10-06

## Context

GraphQL primer, Oct 6. GraphQL is concept-only knowledge: production APIs are REST (representational state transfer). That is the honesty boundary, and it must be stated whenever GraphQL comes up.

## Key points

A GraphQL server acts as a BFF (backend for frontend) with client-owned query shaping. The client decides which fields it wants, and the server shapes the response to that query.

The N+1 problem: one query can trigger one database call per returned item. DataLoader is the standard fix, batching those calls together.

Caching is harder with GraphQL than with REST (representational state transfer), because responses are shaped per query and do not map cleanly onto cache keys.

Query-depth attacks are a risk: a deeply nested query can be expensive to resolve, so depth must be limited.

## Past mistakes & points to improve

- Frame GraphQL as concept knowledge with the BFF (backend for frontend) analogy. The analogy is the honest way to show understanding: a GraphQL server plays the same role as a backend for frontend, with the client shaping the query.
- Never present GraphQL as production experience. Production APIs are REST (representational state transfer), and claiming otherwise would be inventing experience.
- When the N+1 problem comes up, name DataLoader in the same sentence. The problem without the fix reads as half an answer.
