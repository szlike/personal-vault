# Count-and-Say Decode (Backtracking Variant)

**Recorded:** 2026-10-15

## Problem

This is a HackToHire variant of LeetCode 38 (Count and Say), run in reverse, worked on Oct 14-15, 2026. The input is ambiguous, so no single pass works. Each position branches on whether the count is one digit or two digits.

## Rules

- Counts run from 1 to 99.
- Counts have no leading zero.
- Consecutive pairs must have different digits. This is the no-merge rule.

## Approach

Use backtracking: choose, explore, unchoose. At each step, try a one-digit count, explore the rest of the string, then undo the choice and try a two-digit count. `parts.pop()` is the undo step that removes the choice just made so the next branch starts from a clean state.

## How the concept landed

He asked "why part.pop()" and "what is backtracking". The concept needed the maze analogy: try one fork in the maze, hit a dead end, walk back to the fork, and try the other path. Backtracking is the same idea in code.

## Past mistakes & points to improve

- Every mutation in backtracking must be undone before the next branch runs. Arrays need `pop` to undo a push. A mutated primitive poisons only its sibling branch, so it also has to be restored. When a backtracking solution misbehaves, check the undo step first.

## Source

Prepared by Copper bot from Robin's interview practice sessions, September–November 2026. Ask Copper bot for more detail on anything in this note.
