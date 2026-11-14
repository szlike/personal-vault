# 692. Top K Frequent Words

**Recorded:** 2026-10-17

## Pattern

Count frequencies, then sort by frequency descending and word ascending for ties.

## Approach

Count how many times each word appears. Sort the words by frequency from highest to lowest. When two words have the same frequency, order them alphabetically from A to Z.

## Complexity

- Time: O(n log n), dominated by the sort, where n is the number of distinct words.
- Extra space: O(n) for the frequency map.

## Past mistakes & points to improve

- The frequency sort was right, but the tie-break was broken. The comparator `return a > b` returns a boolean, not a number, so the sort order came out wrong. On ["banana","apple","banana","apple","cherry"] with k = 3, the code gave ["banana","apple","cherry"] instead of the correct ["apple","banana","cherry"].
- Sort comparators must return numbers, never booleans. Use subtraction for numbers, or `localeCompare` for strings.
