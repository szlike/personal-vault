# 74. Search a 2D Matrix

**Recorded:** 2026-11-14  
**Original practice:** 2026-10-18

## Pattern

Each row is sorted and the first element of a row is greater than the last of the previous row, so the full `m × n` matrix behaves like one sorted array.

```js
function searchMatrix(matrix, target) {
  if (!matrix.length || !matrix[0].length) return false;
  const rows = matrix.length, cols = matrix[0].length;
  let left = 0, right = rows * cols - 1;
  while (left <= right) {
    const mid = left + Math.floor((right - left) / 2);
    const value = matrix[Math.floor(mid / cols)][mid % cols];
    if (value === target) return true;
    if (value < target) left = mid + 1;
    else right = mid - 1;
  }
  return false;
}
```

## Complexity

Time O(log(mn)), extra space O(1).

## Past mistakes and feedback

- **Observed:** Incorrect midpoint/bound update and mixing rows with columns in virtual-index conversion.
- **Observed:** Hardcoded value `11` appeared where dimensional calculations were needed.
- **Observed:** Empty matrix check and boundary ordering needed tightening.
- **Comparison:** LC 240 has only row/column sorting, not whole-matrix global sorting; the flattened binary search is *not* valid there.

## Points to improve

1. Say “flatten logically without allocating” and derive row=`mid/cols`, col=`mid%cols`.
2. Test a non-square 2×4 matrix; this exposes rows/cols confusion.
3. Use `left <= right` and move bounds by ±1 to guarantee progress.
4. Explain why the LC 74 precondition differs from LC 240.

## Source

- **Documentation assistant:** ChatGPT (OpenAI).
- **Source material:** Robin's prior ChatGPT interview-preparation and AI-learning discussions, consolidated into this study note; not a verbatim transcript.
- **Discussion reference:** Original practice: 2026-10-18.
- **For follow-up:** Share [74. Search a 2D Matrix](https://github.com/szlike/personal-vault/blob/main/leetcode-summary/2026-11-14-0074-search-a-2d-matrix.md) with ChatGPT and ask for a deeper explanation, practice question, or review of past mistakes. Original chat permalinks were not recorded.
