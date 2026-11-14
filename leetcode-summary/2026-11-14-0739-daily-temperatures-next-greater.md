# 739. Daily Temperatures and Next Greater Element

**Recorded:** 2026-11-14  
**Practice period:** October 2026

## Pattern

Use a **monotonically decreasing stack of indices**. When a new value is greater than the value at the stack top, the new index is the answer for the popped index.

```js
function dailyTemperatures(temperatures) {
  const answer = Array(temperatures.length).fill(0), stack = [];
  for (let i = 0; i < temperatures.length; i++) {
    while (stack.length && temperatures[i] > temperatures[stack[stack.length-1]]) {
      const prev = stack.pop();
      answer[prev] = i - prev;
    }
    stack.push(i);
  }
  return answer;
}
```

## Why it works

Each index enters and leaves the stack at most once. The stack holds unresolved days whose temperatures are decreasing from bottom to top. Time O(n), extra space O(n).

## Related patterns

Next greater **value** stores values/indices depending on output requirements; next greater in a **circular** array usually iterates up to `2n` positions and pushes indices only on the first pass.

## Past mistakes and feedback

- **Recovered topic:** Daily Temperatures/Next Greater were in your October interview cheat sheet; no specific implementation error was verified for this problem.
- **Common trap to test:** Storing values when distance `i-prev` requires indices; popping on `>=` when strictly greater is required; quadratic nested scanning.

## Points to improve

1. Explain the stack invariant in one sentence before coding.
2. Dry-run `[73,74,75,71,69,72,76,73]`.
3. Prove amortized O(n) despite nested `while`.

## Source

- **Documentation assistant:** ChatGPT (OpenAI).
- **Source material:** Robin's prior ChatGPT interview-preparation and AI-learning discussions, consolidated into this study note; not a verbatim transcript.
- **Discussion reference:** Practice period: October 2026.
- **For follow-up:** Share [739. Daily Temperatures and Next Greater Element](https://github.com/szlike/personal-vault/blob/main/leetcode-summary/2026-11-14-0739-daily-temperatures-next-greater.md) with ChatGPT and ask for a deeper explanation, practice question, or review of past mistakes. Original chat permalinks were not recorded.
