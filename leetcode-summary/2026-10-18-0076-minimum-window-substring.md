# 76. Minimum Window Substring

**Recorded:** 2026-10-18

**Status:** OPEN

## Pattern

Sliding window with two pointers, built on the LeetCode 438 machinery.

## Approach

This problem takes the LeetCode 438 machinery and makes the window breathe instead of sliding at a fixed size. The 26-slot count arrays, the diff counter, and the O(1) updates per slide are the prerequisite. The right edge expands to include characters, and the left edge shrinks whenever the window already contains the target.

## Complexity

- Time: O(n), because each character enters and leaves the window once.
- Extra space: O(1), because the count arrays have a fixed size.

## Past mistakes & points to improve

- None yet. No attempt was made, so there are no mistakes to record. The attempt itself is the open item.
- Status: OPEN. Hints only were given on October 18, and no attempt from him was found through November 13.
