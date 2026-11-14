# Dynamic Programming, Memoization, and 0/1 Knapsack

**Recorded:** 2026-11-14  
**Practice period:** September–October 2026

## When DP applies

Look for overlapping subproblems and optimal substructure. Define **state**, **transition**, **base case**, **evaluation order**, and **answer location** before writing code.

## 0/1 knapsack

Given item weights `w[i]`, values `v[i]`, and capacity `C`, choose each item at most once. One-dimensional DP:

```js
function knapsack(weights, values, capacity) {
  const dp = Array(capacity + 1).fill(0);
  for (let i = 0; i < weights.length; i++) {
    for (let c = capacity; c >= weights[i]; c--) {
      dp[c] = Math.max(dp[c], dp[c - weights[i]] + values[i]);
    }
  }
  return dp[capacity];
}
```

Time O(nC), space O(C). Descending capacity iteration prevents using an item more than once; ascending order would describe an unbounded/reusable-item variant.

## Connection to longest palindrome

A recursive palindrome attempt can recompute the same `(left,right)` range exponentially. Memoization caches states; center expansion is often still the better interview solution for LC 5 because it is simpler and uses constant extra space.

## Past mistakes and feedback

- **Observed in palindrome practice:** Repeated recursive calls and incomplete base cases; this was concrete evidence of needing stronger recurrence design.
- **Recovered study topic:** 0/1 knapsack was reviewed; no verified specific bug in your knapsack code was found.
- **Potential trap:** Conflating recursion, backtracking and DP. Backtracking enumerates choices; DP merges repeated states when valid.

## Points to improve

1. Write state and recurrence as a sentence before implementation.
2. Explain why 0/1 uses descending capacity.
3. Compare brute force O(2ⁿ), memoized state count and bottom-up DP.
4. Dry-run capacity 5 with items `(weight,value)=(2,3),(3,4),(4,5)`.

## Source

- **Documentation assistant:** ChatGPT (OpenAI).
- **Source material:** Robin's prior ChatGPT interview-preparation and AI-learning discussions, consolidated into this study note; not a verbatim transcript.
- **Discussion reference:** Practice period: September–October 2026.
- **For follow-up:** Share [Dynamic Programming, Memoization, and 0/1 Knapsack](https://github.com/szlike/personal-vault/blob/main/leetcode-summary/2026-11-14-dynamic-programming-and-knapsack.md) with ChatGPT and ask for a deeper explanation, practice question, or review of past mistakes. Original chat permalinks were not recorded.
