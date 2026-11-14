# 5. Longest Palindromic Substring

**Recorded:** 2026-11-14  
**Original practice:** 2026-09-14; revisited 2026-10-19

## Pattern

Expand around every possible palindrome center: `(i, i)` for odd length and `(i, i+1)` for even. Keep the longest **contiguous substring**.

```js
function longestPalindrome(s) {
  let bestStart = 0, bestLen = 0;
  const expand = (left, right) => {
    while (left >= 0 && right < s.length && s[left] === s[right]) {
      left--; right++;
    }
    const len = right - left - 1;
    if (len > bestLen) { bestLen = len; bestStart = left + 1; }
  };
  for (let i = 0; i < s.length; i++) {
    expand(i, i);
    expand(i, i + 1);
  }
  return s.slice(bestStart, bestStart + bestLen);
}
```

## Why it works and complexity

Every palindrome has one middle character or middle gap. Expansion tests exactly matching boundaries, so resulting characters are adjacent in the input. O(n²) time in the worst case; O(1) extra space excluding output.

**Advanced:** Manacher transforms the input to handle even/odd centers uniformly, reuses the palindrome radius of a mirror around the current center, and achieves O(n) time/O(n) space. Explain center expansion confidently first.

## Past mistakes and feedback

- **Observed code bug:** Missing empty/length-1 base case in recursive approach led to runaway recursion and maximum call stack.
- **Observed JavaScript bug:** Used `s.length = 1` (assignment) instead of `s.length === 1` (comparison).
- **Observed algorithm bug:** Concatenating matching endpoints with an interior result could produce a palindrome that was **not a substring** of original input (e.g. invalid `abba` from `abbca`).
- **Observed complexity:** Duplicated recursive subproblems, exponential growth without memoization; center expansion was simpler and safer.

## Points to improve

1. Before recursion, identify state, base case and proof of progress.
2. Clarify **substring** vs subsequence before choosing a recurrence.
3. Dry-run `''`, `'a'`, `'aa'`, `'abba'`, `'babad'`, `'abbca'`.
4. Explain why successful expansion length is `right-left-1` after the final failed comparison.

