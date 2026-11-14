# 240. Search a 2D Matrix II

**Recorded:** 2026-11-13

## Source

**Prepared by:** Codex (OpenAI)

**Basis:** Current repository content and standard algorithmic reasoning for this problem. The mistakes and drills are inferred from the note’s documented lessons, not from inaccessible prior conversation history.

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

## Past mistakes and lessons

- **Stopping when a single move is unavailable.** At the top-right corner, moving left can be blocked while moving down remains valid (and the reverse can occur later). The loop ends only when the current position itself leaves the matrix.
- **Using the wrong boundary for a rectangular matrix.** Rows and columns must be checked independently: `row < matrix.length` and `col >= 0`. Do not assume a square matrix or use the row count for both dimensions.
- **Skipping the empty-input guard.** `matrix[0]` is unsafe when the matrix is empty. Establish `matrix.length === 0 || matrix[0].length === 0` before reading dimensions.
- **Explaining the move without the invariant.** The key proof is that, at `(row, col)`, values below are at least the current value and values left are at most it. One comparison eliminates an entire row or column segment.
- **Choosing recursion when an iterative scan is clearer.** Recursion is valid, but it adds stack space and makes the stopping conditions easier to misstate in an interview.

## Improvements and practice

1. State the invariant before coding: the remaining candidate region is rows `row..m-1` and columns `0..col`.
2. Dry-run a non-square matrix, a one-row matrix, a one-column matrix, an absent target, and an empty matrix.
3. Write the iterative version from memory, then explain why each step removes one impossible row or column.
4. Contrast the (O(m+n)) approach with scanning (O(mn)) and binary-searching every row (O(m\log n)); say why the corner walk best uses both sort orders.
