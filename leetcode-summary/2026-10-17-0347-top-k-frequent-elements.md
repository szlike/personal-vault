# 347. Top K Frequent Elements

**Recorded:** 2026-10-17

## Pattern

Bucket sort by frequency.

## Approach

Count how many times each number appears. Create one bucket per frequency value, place each number in the bucket matching its frequency, then collect numbers from the highest buckets down until k numbers are gathered.

## Complexity

- Time: O(n), where n is the input length.
- Extra space: O(n) for the frequency map and the buckets.

## Past mistakes & points to improve

- The bucket-sort version was already O(n), but it allocated `n + 1` buckets. Allocating `maxFreq + 1` buckets instead, sized to the actual maximum frequency, cut the runtime from 62.8 ms to 15.6 ms on n = 200,000, about a 4x improvement.
- Size buckets to the actual max frequency, not the input length.
