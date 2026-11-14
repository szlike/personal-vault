# 438. Find All Anagrams in a String

**Recorded:** 2026-10-13

## Pattern

Fixed-size sliding window with two 26-slot count arrays, one for the pattern and one for the window.

## Approach

Count the letters of the pattern `p` into one 26-slot array. Slide a window of the same length across the string `s`, counting its letters into a second 26-slot array. Each slide changes only two slots: the character leaving the window and the character entering it, so each update costs O(1) time.

A lemma came up during the session: if the leaving character equals the entering character, the window stays an anagram. That is correct but incomplete, because it cannot bootstrap from a window that is not an anagram in the first place. On s = "cbaebabacd" with p = "abc", the first window "cba" is an anagram, but the reasoning has no way to recover once a window stops being one.

The version that needs no rescan keeps a diff counter: a single number that tracks how many of the 26 slots differ between the two count arrays. When the counter is zero, the window is an anagram. Updating the counter costs O(1) time per slide.

Char-code sums are not a shortcut. They are lossy: "ad" and "bc" both sum to 197, but they are not anagrams of each other.

## Complexity

- Time: O(n), because each character is visited once and each slide costs O(1) time.
- Extra space: O(1), because the count arrays have a fixed size of 26.

## Past mistakes & points to improve

- The first version was O(n times m squared): it called `t.replace` for every character in every window. The fix is 26-slot count arrays with two O(1) updates per slide.
- Never rescan the whole window to check for an anagram. Maintain a diff counter and update it on each slide instead.
