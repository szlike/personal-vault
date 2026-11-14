# 43. Multiply Strings

**Recorded:** 2026-10-15

## Pattern

Positional multiplication into a result array, one digit at a time, without converting the inputs to numbers.

## Approach

The canonical solution uses a result array. For each pair of digits `num1[i]` and `num2[j]`, multiply them and drop the ones digit into `result[i+j+1]` and the tens digit into `result[i+j]`. The full solution was traced line by line on 123 times 456.

## Complexity

- Time: O(m times n), where m and n are the lengths of the two input strings.
- Extra space: O(m + n) for the result array.

## Past mistakes & points to improve

- Two bugs in the first version. First, a crash: the final `reduce` blew the heap on large inputs. Second, corruption: a digit-string bug inside the loop.
- The fixed version still failed one edge case: anything times 0 returned an empty string `""` instead of `"0"`. The line `"0".replace(/^0+/, '')` eats the only character, so the result is empty. Strip leading zeros only when the result has more than one character, or handle the all-zeros case separately.
- This is the highest-risk bug class in the notes so far: a digit string must never become a loop count or a JavaScript number. Keep digits as strings or as single digits in an array, and convert only one digit at a time.
