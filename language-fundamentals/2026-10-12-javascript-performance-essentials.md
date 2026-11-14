# JavaScript Performance Essentials

**Recorded:** 2026-10-12

## Context

Measured study distilled from the October 11 to 12 LeetCode sessions (01 Matrix, Rotting Oranges, Surrounded Regions). Every claim below was measured, not assumed.

> Big-O tells you whether it scales; the constant factors tell you whether it ships.

## Allocation is the number one tax in JavaScript

**Headline rule, bolded at Robin's request: allocation is the number one tax in JavaScript. Hoist literals out of loops, int-encode queue entries, never rebuild direction arrays per pop.**

- Hoist literals out of loops. Rebuilding `[[0,1],[0,-1],[1,0],[-1,0]]` on every queue pop was one of the largest hidden costs measured.
- Int-encode queue entries as `x * n + y`, and never push `[x, y]` pairs in a hot loop. One pair means one heap object per cell, plus garbage collector cleanup later.
- Destructuring and pair arrays in hot paths cost twice: once to pay for the array, and once more to unpack it. Plain indexed reads are free by comparison.

## Data structure choice

- `shift()` becomes a head pointer. Measured 100 times slower on Rotting Oranges: 747.6 ms down to 7.4 ms. `shift()` moves the whole backing array in memory on every pop.
- Visited tracking: in-place marking is better than a boolean array, which is better than a Set. Array get beat `Set.has` 3.6 times on 500,000 scattered lookups: 13.5 ms versus 48.8 ms.
- The queue is for order and the mark is for membership: one tool per job. A Set doing double duty as a queue breaks the moment level boundaries are needed, because BFS (breadth-first search) levels are minutes in Rotting Oranges.

## Access pattern

- Sequential access beats scattered access, always. The row-major sweeps of dynamic programming versus the wavefront of BFS (breadth-first search) is structural: it cannot be rewritten away, only recognized, so you know which side you are on.
- Division and modulo per cell is real but minor: about 2 times the leftover gap, never the 100 times term. See the encode tradeoff note below.

## Measurement discipline

- Benchmark against your corrected code. Comparing a fast solution against your own buggy version nearly taught the wrong lesson, that `shift()` was fine.
- LeetCode milliseconds at tiny constraints are noise. At 10 by 10, every variant runs in under a millisecond. Trust the structure, not the number.

## The scariest bug class

- `head++` instead of `head += loop`: correct answer, quadratic time. It passes every test and dies in production. It was caught by counting, not by reading. Measured: 35,558 ms down to 7.4 ms from this one-line fix, about 4800 times faster.

## Encode tradeoff (int versus pair arrays)

Int-encoding (`x * n + y`) trades per-cell allocation plus garbage collection for per-cell division plus modulo on decode. Allocation almost always dominates in V8, the JavaScript engine, so int-encoding won when measured, but it is a tradeoff, not a law. The third option dodges both costs: push x and y as two flat entries with `q.push(x, y)`, and pop with `head += 2`. No pair array, no division. Measured three-way on 01 Matrix BFS (breadth-first search) at 300 by 300: pairs 16.5 ms, int-encode 7.2 ms, flat push 8.4 ms. Int-encode and flat-push both crush pairs. Pick int-encode for compactness, or flat-push if you want zero division.

## Evidence log (all measured locally, V8)

- 01 Matrix BFS (breadth-first search): pair queue 38.1 ms down to int queue 7.7 ms, 5 times faster; dynamic programming 4.2 ms.
- Rotting Oranges: the `head++` bug 35,558 ms down to `head += loop` 7.4 ms, about 4800 times faster; the `shift()` plus pairs version 747.6 ms, 100 times slower than the fixed head-pointer version.
- Surrounded Regions: Set version 8.4 ms down to in-place `'S'` marking 2.6 ms, about 3 times faster.
- Array get versus `Set.has`: 13.5 ms versus 48.8 ms on 500,000 scattered lookups, 3.6 times faster.

## Past mistakes & points to improve

- Benchmark against your corrected code. Comparing a fast solution against your own buggy version nearly taught the wrong lesson, that `shift()` was fine.
- LeetCode milliseconds at tiny constraints are noise. Trust the structure, not the number.
- The scariest bug class is `head++` instead of `head += loop`: correct answer, quadratic time, passes every test, dies in production. It was caught by counting, not by reading. When a solution feels slow, instrument the code and count, instead of reading it again.

## Source

Prepared by Copper bot from Robin's interview practice sessions, September–November 2026. Ask Copper bot for more detail on anything in this note.
