# 20. Valid Parentheses and Stack Basics

**Recorded:** 2026-11-14  
**Practice period:** October 2026

## Pattern

Push expected closing brackets when an opener appears; when a closer appears, it must match the most recent expectation. Finish with an empty stack.

```js
function isValid(s) {
  const expect = { '(':')', '[':']', '{':'}' };
  const stack = [];
  for (const ch of s) {
    if (ch in expect) stack.push(expect[ch]);
    else if (stack.pop() !== ch) return false;
  }
  return stack.length === 0;
}
```

## Complexity

Time O(n); extra space O(n) worst case.

## Past mistakes and feedback

- **Recovered topic:** Stack basics were included in our October algorithm cheat sheet; no verified specific user bug for this exact problem was retrieved.
- **Common traps to test:** Closing bracket on empty stack, extra unmatched opening brackets, and checking only counts rather than nesting order.

## Points to improve

1. Explain why LIFO is essential (`([)]` is invalid even though counts match).
2. Dry-run empty input, `()[]{}`, `([)]`, `(((` and `]`.
3. Connect ordinary stack matching to **monotonic** stacks: both use LIFO, but a monotonic stack also maintains value order.

