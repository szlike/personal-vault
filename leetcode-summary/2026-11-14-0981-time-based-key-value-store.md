# 981. Time Based Key-Value Store (TimeMap)

**Recorded:** 2026-11-14  
**Original practice:** 2026-10-18

## Pattern

Map each key to timestamp-sorted pairs. `set` appends because the problem guarantees timestamps are strictly increasing for each key. `get(key,t)` returns the value with the greatest timestamp `<= t`; use **rightmost-valid binary search**.

```js
class TimeMap {
  constructor() { this.map = new Map(); }
  set(key, value, timestamp) {
    if (!this.map.has(key)) this.map.set(key, []);
    this.map.get(key).push([timestamp, value]);
  }
  get(key, timestamp) {
    const entries = this.map.get(key) ?? [];
    let left=0, right=entries.length-1, answer='';
    while (left <= right) {
      const mid = left + Math.floor((right-left)/2);
      if (entries[mid][0] <= timestamp) { answer=entries[mid][1]; left=mid+1; }
      else right=mid-1;
    }
    return answer;
  }
}
```

## Complexity

`set` amortized O(1); `get` O(log m) for m timestamps stored for the key; storage O(total entries).

## Past mistakes and feedback

- **Observed:** Proposed filtering/sorting the values at query time; unnecessary and slower because timestamps are already ordered.
- **Observed:** Comparator in JavaScript numeric sort needed correct arithmetic (`a-b` rather than boolean return).
- **Observed:** Missing earlier timestamp could yield `undefined` instead of required empty string.
- **Feedback:** Binary search must return the **last** `<= target`, not simply an exact match.

## Points to improve

1. State the monotonic timestamp guarantee and when it would not hold in a real system.
2. Dry-run timestamps 1,4,7 and queries 0,1,3,4,10.
3. Write a rightmost-valid binary-search template from memory.

