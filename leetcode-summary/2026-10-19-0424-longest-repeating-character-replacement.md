# 424. Longest Repeating Character Replacement

**Recorded:** 2026-10-19

**Status:** OPEN (re-verify)

## Pattern

Sliding window where window length minus the most-frequent character count equals the replacements needed.

## Approach

Keep a window and a count of each character inside it. Track `maxFreq`, the count of the most frequent character in the window. The number of replacements needed to make the whole window one character is window length minus `maxFreq`. If that number is larger than k, shrink the window from the left until it fits again. The canonical window was taught line by line and walked on "AABAB".

`maxFreq` is never decremented. It is read in exactly one place, the shrink test, so a stale value can only make the window shrink later than necessary, never wrongly. This was measured over 300,000 random strings: every time the window length reached a new high, `maxFreq` equaled the window's true maximum.

## Complexity

- Time: O(n), because each character enters and leaves the window once.
- Extra space: O(1), because the count array has a fixed size of 26.

## Past mistakes & points to improve

- October 17 attempt: a per-letter gap-counting approach. It passes both LeetCode examples but fails "aaaYYYYaaa" with k = 1, returning 6 when the true answer is 5, because "YYYYa" can become all Y. Greedy spends the replacement budget on early gaps and cannot recover. A reset cannot fix it either: on "BBBABAAABBBB" with k = 3 it gives 7 when the true answer is 8.
- October 19 rewrite: one bug in the shrink step, the wrong index. The code wrote `count[s[i]]--`, which decrements the newcomer, but `left++` removes the leftmost character, so it must be `count[s[left]]--`. On "AABABBA" with k = 1 it returns 5 when the correct answer is 4.
- A lazy `maxFreq--` on shrink hangs forever: on "ABAB" with k = 1 the window never exits after 100,000 shrinks.
- Shrink-step bookkeeping rule: uncount the departing leftmost character, never the newcomer. Phantom counts inflate the validity check against a ghost window while the length uses the real one.
- Status: OPEN. The corrected version was not re-pasted or verified after October 19, so re-verify it before treating this problem as done.
