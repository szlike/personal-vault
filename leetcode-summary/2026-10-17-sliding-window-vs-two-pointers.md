# Sliding Window vs Two Pointers

**Recorded:** 2026-10-17

## Concept

A sliding window is a special case of two pointers. Every sliding window uses two pointers, but not every two-pointer solution is a sliding window. The difference is the movement rule. In a sliding window, the two pointers mark the two ends of one window that grows and shrinks over the data. In a general two-pointer solution, the pointers can move by a different rule, for example one from each end moving toward each other.

## Past mistakes & points to improve

- Name the pattern from the movement rule, not from the shape of the code. If the two pointers always bound a single window, it is a sliding window. If they move by another rule, it is two pointers but not a sliding window.
