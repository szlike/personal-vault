# Two Pointers, Linked Lists, and Merging Sorted Arrays

**Recorded:** 2026-11-14  

## Source

**Prepared by:** Codex (OpenAI)

**Basis:** Consolidated interview-preparation material in this repository. Treat examples, metrics, and ownership as prompts to verify against your firsthand experience; return to Codex with the note title for expansion or correction.
**Practice period:** October 2026

## Sorted pair sum

For a sorted array, put `left` at start and `right` at end. If sum too small, increase left; if too large, decrease right. O(n) time/O(1) extra space. This relies on sortedness; do not apply this reasoning to arbitrary unsorted input.

## Linked-list middle and cycle detection

- **Middle:** `slow=slow.next`, `fast=fast.next.next`; when fast reaches the end, slow is at middle. Define which middle is expected for even length.
- **Cycle:** move slow one and fast two; if they meet, a cycle exists. Handle null checks before dereferencing.
- **Cycle entry (advanced):** after collision, reset one pointer to head and advance both one step until meeting.

## Merge sorted arrays

If first array has spare capacity at its end (common LC 88 variant), fill **from the back**, comparing the largest unmerged elements, so you don't overwrite input values. Otherwise classic two-pointer merge uses O(m+n) time and output space.

## Past mistakes and feedback

- **Recovered topic:** Two pointers, linked-list middle/cycle and sorted merge were included in the October cheat sheet, but a specific user error for each wasn't recovered.
- **General observed bug class:** Off-by-one and boundary conditions showed up in nearby algorithm exercises.
- **Potential trap:** Treating a linked-list node like an array index or accessing `fast.next.next` without a valid guard.

## Points to improve

1. Derive pointer movement from monotonicity; don't memorize moves without a reason.
2. Dry-run 0, 1, 2, 3, 4-node linked lists.
3. Show why backward merge prevents overwriting unprocessed values.

## Source

- **Documentation assistant:** ChatGPT (OpenAI).
- **Source material:** Robin's prior ChatGPT interview-preparation and AI-learning discussions, consolidated into this study note; not a verbatim transcript.
- **Discussion reference:** Practice period: October 2026.
- **For follow-up:** Share [Two Pointers, Linked Lists, and Merging Sorted Arrays](https://github.com/szlike/personal-vault/blob/main/leetcode-summary/2026-11-14-two-pointers-linked-lists-and-merge.md) with ChatGPT and ask for a deeper explanation, practice question, or review of past mistakes. Original chat permalinks were not recorded.
