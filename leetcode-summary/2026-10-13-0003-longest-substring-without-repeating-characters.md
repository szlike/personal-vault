# 3. Longest Substring Without Repeating Characters

**Recorded:** 2026-10-13

## Pattern

Sliding window with a map from each character to its last seen index.

## Approach

Keep a left edge for the window and a map from each character to the last index where it appeared. When a character repeats inside the current window, move the left edge just past its previous occurrence. Measure the window length at each step, and measure the final window once more after the loop ends.

The guard on the repeat must be `lastIndex >= left`. Using `> left` instead misses a repeat that sits exactly at the left edge. On the string "abac", the wrong guard returns 4 instead of the correct answer 3.

## Complexity

- Time: O(n), because each character is visited once.
- Extra space: O(min(n, alphabet size)) for the map.

## Past mistakes & points to improve

- Both early solutions were too slow. The first was O(n cubed): it used `str.includes` on every substring and copied strings. The second was O(n squared): it used `temp.indexOf` plus rewind arithmetic. The fix is one pass with a map from each character to its last index.
- The `>=` in the guard is load-bearing. A repeat exactly at the left edge still counts as a repeat inside the window, so `> left` silently lets it through.
- Always measure the final window after the loop ends. The trailing window is never measured inside the loop, so skipping that step loses the answer when the longest substring runs to the end of the string.
