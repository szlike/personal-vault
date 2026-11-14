# 128. Longest Consecutive Sequence

**Recorded:** 2026-11-14  

## Source

**Prepared by:** Codex (OpenAI)

**Basis:** Consolidated interview-preparation material in this repository. Treat examples, metrics, and ownership as prompts to verify against your firsthand experience; return to Codex with the note title for expansion or correction.
**Original practice:** 2026-10-17

## Pattern

Store numbers in a Set; begin counting **only when `x-1` is absent**, meaning `x` starts a run.

```js
function longestConsecutive(nums) {
  const values = new Set(nums);
  let best = 0;
  for (const start of values) {
    if (values.has(start - 1)) continue;
    let length = 1;
    while (values.has(start + length)) length++;
    best = Math.max(best, length);
  }
  return best;
}
```

## Complexity

Expected O(n) time and O(n) space. Each run is expanded only from its first element.

## Past mistakes and feedback

- **Observed:** Sorting produced O(n log n), violating the expected linear-time target.
- **Observed:** Duplicate numbers and empty input required explicit handling in the earlier logic.
- **Observed misconception:** This isn't a sliding-window problem on the original array; consecutiveness is by **numeric value**, independent of input order.

## Points to improve

1. State the “only starts of sequences” invariant before coding.
2. Test `[100,4,200,1,3,2]`, `[1,2,0,1]`, `[]`, negative numbers.
3. Explain expected Set complexity and what duplicates do to iteration.

## Source

- **Documentation assistant:** ChatGPT (OpenAI).
- **Source material:** Robin's prior ChatGPT interview-preparation and AI-learning discussions, consolidated into this study note; not a verbatim transcript.
- **Discussion reference:** Original practice: 2026-10-17.
- **For follow-up:** Share [128. Longest Consecutive Sequence](https://github.com/szlike/personal-vault/blob/main/leetcode-summary/2026-11-14-0128-longest-consecutive-sequence.md) with ChatGPT and ask for a deeper explanation, practice question, or review of past mistakes. Original chat permalinks were not recorded.
