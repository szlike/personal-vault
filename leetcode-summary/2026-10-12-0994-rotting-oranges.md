# 994. Rotting Oranges

**Recorded:** 2026-10-12

## Problem

A grid holds empty cells (0), fresh oranges (1), and rotten oranges (2). Every minute, each rotten orange rots its fresh neighbors in the 4 directions. Return the minutes until no fresh orange remains, or -1 if some fresh orange can never rot. LeetCode problem 994.

## Pattern

Multi-source breadth-first search (BFS). Seed the queue with every rotten orange at the start, then process the queue level by level. Each level is one minute. Count the fresh oranges up front and stop when the count reaches zero.

## Attempts

Three attempts plus a redo on October 10 to 12.

Attempt 1 had five bugs:

- `for...in` on an array yields string indices, so destructuring `[nx, ny]` from one gave garbage values.
- `grid[nx, ny]` uses the comma operator, which reads `grid[ny]` instead of the cell at row `nx`, column `ny`.
- The neighbor loop used `return` where it needed `continue`, so one bad neighbor abandoned the whole search.
- `freshCount` was never decremented when a fresh orange rotted, so the count never reached zero.
- Rotted oranges were marked as `0`, which already means empty, while the guard only skipped `0`. The mark collided with a real cell state.

Attempt 2 still had the comma operator in the guard, so the same indexing bug survived the fix. It also missed the two-phase restructure: scan and collect first, then run the breadth-first search. The algorithm was run inside the counting scan instead of after it.

Redo on October 12: the full skeleton was correct from memory. It had multi-source seeding, levels counted as minutes, and fresh counting. Two bugs remained. The visited-marker collision bug was still there. Also, after processing a level, the queue head must advance by `loop`, the number of cells processed in that level, not by one. The code used `head++` where it needed `head += loop`.

Resolved on October 12. Both his version and the reference version were verified correct, and his version ran about 100 times faster on a large grid.

## Complexity

- Time: each cell enters the queue once, so the time grows with the number of cells in the grid.
- Extra space: the queue holds at most the number of cells in the grid.

## Key insight

Separate the counting scan from the search. First scan the grid, count the fresh oranges, and collect the rotten ones. Only then run the breadth-first search. Mixing the two phases together is what hid the bugs.

## Past mistakes & points to improve

- October 10, attempt 1: `for...in` on an array yields string indices. Fix: use `for...of` for values and `for...in` only for keys.
- October 10, attempt 1: `grid[nx, ny]` reads `grid[ny]` because of the comma operator. Fix: always write `grid[nx][ny]`, and when one instance is fixed, search the function for its siblings.
- October 10, attempt 1: `return` instead of `continue` in the neighbor loop. Fix: a bad neighbor means skip that neighbor only. `return` exits the whole function.
- October 10, attempt 1: `freshCount` never decremented. Fix: update every counter at the line where the change happens.
- October 10, attempt 1: marking rotted oranges as `0` collided with empty cells. Fix: never reuse a value that already means something else as a visited marker.
- October 10, attempt 2: the comma operator survived in the guard after the first fix. Fix: when a bug is found, search the whole function for other instances of the same bug.
- October 10, attempt 2: no two-phase restructure. Fix: scan and collect first, then run the breadth-first search. Never run the main algorithm inside the counting scan.
- October 12, redo: `head++` after a level must be `head += loop`, because `loop` cells were processed, not one. Fix: advance the queue head by the number of cells actually processed.
- Standing rule from this problem: separate the counting scan from the breadth-first search, and never reuse a value (0) that already means something else as a visited marker.
