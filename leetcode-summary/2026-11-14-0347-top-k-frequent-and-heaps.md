# 347. Top K Frequent Elements and Heap Fundamentals

**Recorded:** 2026-11-14  
**Original practice:** 2026-10-17/19

## Pattern

Count frequencies with Map. Then either:

- **Bucket sort:** index buckets by frequency 1..n, scan descending until k values collected. Expected O(n) time and O(n) space.
- **Size-k min-heap:** keep the k most frequent values seen so far, O(n log k) after counting; useful when k is small or elements stream in.

## Heap mechanics

In array-backed binary heap, parent of index `i` = `Math.floor((i-1)/2)`, children = `2*i+1`, `2*i+2`. On insertion, **shift/sift up** swaps with parent until heap order restored; on root removal, move last element to root then **shift/sift down**. `push`/`pop` O(log n); bottom-up heapify O(n).

## Example sift-up (max-heap)

Insert 12 into `[10,7,5]` → append `[10,7,5,12]` → compare with parent 7 → `[10,12,5,7]` → compare with parent 10 → `[12,10,5,7]`.

## Past mistakes and feedback

- **Observed:** You asked what `shiftUp` means and how parent/child indexes work.
- **Recovered cheat-sheet mistakes:** Heap child bounds, root return and numeric comparator issues were recurring things to test.
- **Potential trap:** A max-heap and a size-k min-heap solve related but different parts of Top-K; explain why you choose one.

## Points to improve

1. Implement a small heap without a library and test 0, 1, 2, duplicate and negative values.
2. State why bottom-up heapify is O(n), not O(n log n).
3. Contrast bucket vs heap by complexity and input constraints.
4. Trace sift-up/sift-down by hand during an interview.

