# 392. Is Subsequence

**Recorded:** 2026-10-10

## Pattern

Use two pointers, one on each string. Walk through the longer string once and advance the pointer on the shorter string only when the characters match. Each pointer moves forward at most once, so the scan is a single pass over both strings.

## Complexity

- Time: \(O(n + m)\), where n and m are the lengths of the two strings.
- Extra space: \(O(1)\).

## Past mistakes & points to improve

- On 2026-10-10, the complexity was stated as \(O(n^2)\). That was wrong. The two-pointer scan is \(O(n + m)\) because each pointer moves forward at most once. State complexity from the loop structure, not from intuition. Count how far each pointer can move, then add the bounds.

## Source

Prepared by Copper bot from Robin's interview practice sessions, September–November 2026. Ask Copper bot for more detail on anything in this note.
