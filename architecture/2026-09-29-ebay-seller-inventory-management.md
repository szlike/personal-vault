# eBay Seller Inventory Management (Architecture Round, Anton Ganeshalingam)

**Recorded:** 2026-09-29

## Context

eBay Architecture round, real loop round with Anton Ganeshalingam, Sep 29. Design seller inventory management.

## Design

The oversell guard is an atomic `UPDATE ... WHERE stock > 0`. The write itself enforces the invariant: stock can never go below zero because the update only succeeds while stock is positive.

The shard key must enclose the transaction boundary. The data that one transaction touches must live on one shard, or the transaction cannot be atomic.

Reads may lag, but the write guard is authoritative. A stale read can show old stock, and that is acceptable, because the atomic write is what actually prevents overselling.

Order events are async, with idempotent consumers keyed on order_id. The same order event can be delivered more than once, and the consumer must handle the repeat safely by checking the order ID.

## Grade

Grade B.

## Past mistakes & points to improve

- Lead with the atomic write guard. The `UPDATE ... WHERE stock > 0` statement is the core of the design, so it belongs in the first minute, not buried after the read path.
- Shard key and transaction boundary must be designed together. Choosing the shard key without checking what one transaction touches risks splitting a transaction across shards, where it can no longer be atomic.
- Say out loud that reads may lag and why that is acceptable. The lag is safe only because the write guard, not the read, enforces the invariant.

## Source

Prepared by Copper bot from Robin's interview practice sessions, September–November 2026. Ask Copper bot for more detail on anything in this note.
