# 240. Search a 2D Matrix II

**Recorded:** 2026-11-13

## Pattern

Search from the top-right cell in a matrix with rows and columns sorted in ascending order.

- Current value too large → move left.
- Current value too small → move down.
- Match → return `true`.

## Complexity

- Time: \(O(m + n)\)
- Extra space: \(O(1)\) with a loop

## Lessons

- The recursive implementation has the same time complexity but uses call-stack space and adds function-call overhead.
- Use an out-of-bounds base case for recursion: `row >= rows || col < 0`.
- Do not stop just because one direction is blocked; the other direction may still contain the target.
