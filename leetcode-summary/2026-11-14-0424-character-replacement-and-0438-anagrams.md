# 424/438. Sliding Window — Character Replacement and Anagrams

**Recorded:** 2026-11-14  
**Original practice:** October 2026

## LC 424 — Longest Repeating Character Replacement

Maintain a window `[left,right]`, character frequencies and highest frequency `maxCount`. A window needs `windowLength-maxCount` replacements to become one repeated character. If greater than `k`, move left. Track largest valid length. O(n) time for fixed alphabet; O(1) alphabet counters.

## LC 438 — Find All Anagrams in a String

Maintain frequency arrays for `p` (need) and the current fixed-length window of `s` (have). Compare the first window, then at every step **subtract the exiting character, add the entering character**, compare again and record the start index.

```js
function findAnagrams(s, p) {
  const n = s.length, m = p.length, result = [];
  if (m > n || m === 0) return result;
  const need = Array(26).fill(0), have = Array(26).fill(0);
  const idx = ch => ch.charCodeAt(0) - 97;
  for (let i = 0; i < m; i++) {
    need[idx(p[i])]++;
    have[idx(s[i])]++;
  }
  const equal = () => need.every((count, i) => count === have[i]);
  if (equal()) result.push(0);
  for (let right = m; right < n; right++) {
    have[idx(s[right - m])]--;
    have[idx(s[right])]++;
    if (equal()) result.push(right - m + 1);
  }
  return result;
}
```

## Past mistakes and feedback

- **Observed question:** “Why push to `win`?” The first loop fills **both** target frequencies and the initial `s` window so the first comparison is meaningful.
- **Observed follow-up:** “How do we compare when the window moves?” Update counts incrementally, do not rebuild arrays each step.
- **Potential traps:** Off-by-one in `right-m+1`, forgetting to compare first window, and assuming ASCII lowercase when input requirements differ.

## Points to improve

1. Draw one fixed-size and one variable-size sliding window and explain when each applies.
2. Trace `s='cbaebabacd', p='abc'` to result `[0,6]`.
3. For 424, explain why length minus most-frequent count determines replacement cost.
4. Practice boundary tests including `p` longer than `s`, equal lengths and repeated letters.

## Source

- **Documentation assistant:** ChatGPT (OpenAI).
- **Source material:** Robin's prior ChatGPT interview-preparation and AI-learning discussions, consolidated into this study note; not a verbatim transcript.
- **Discussion reference:** Original practice: October 2026.
- **For follow-up:** Share [424/438. Sliding Window — Character Replacement and Anagrams](https://github.com/szlike/personal-vault/blob/main/leetcode-summary/2026-11-14-0424-character-replacement-and-0438-anagrams.md) with ChatGPT and ask for a deeper explanation, practice question, or review of past mistakes. Original chat permalinks were not recorded.
