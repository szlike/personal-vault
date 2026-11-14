# 1055. Shortest Way to Form String

**Recorded:** 2026-10-13

## Pattern

Subsequence matching with a forward position cursor into the source string.

## Approach

Walk through the target string one character at a time and search forward in the source string from the current position, using `indexOf(ch, p)`. After a match, set the cursor to `prevId = index + 1`, so the next search starts after the matched character. When the search returns -1, the current subsequence is exhausted: start a new subsequence and count one more.

The detector for a wrap is `<=`: if the new index is less than or equal to the previous cursor, the same slot was reused, which means a new subsequence has started.

Calling `indexOf` from 0 on every character ignores order. A counter cannot fix a positional constraint; only a position cursor can.

## Complexity

- Time: O(n times m) in the worst case, where n is the target length and m is the source length.
- Extra space: O(1).

## Past mistakes & points to improve

- The first attempt searched from index 0 every time, which ignores the order of the characters. The fix is a forward search with `indexOf(ch, p)` and a cursor that only moves forward.
- The -1 marker idea was sound but undercounted: it pushed a marker without resolving the fresh index for the character that triggered the wrap.
- Subsequence problems need a position cursor, not a count. A count can say how many characters matched, but it cannot say where the next match is allowed to start.

## Source

Prepared by Copper bot from Robin's interview practice sessions, September–November 2026. Ask Copper bot for more detail on anything in this note.
