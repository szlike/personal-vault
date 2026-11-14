# Grid BFS/DFS Bug Checklist

**Recorded:** 2026-10-11

## Purpose

Run this checklist before submitting any grid breadth-first search (BFS) or depth-first search (DFS) solution. Every item below is a bug actually shipped in the October 9 to 11 prep sessions, not generic advice.

## Loop and syntax traps

- **Return versus continue in neighbor loops.** A bad neighbor means `continue`, which skips only this neighbor. `return` abandons the whole search and skips any code below it in the function. Seen in Rotting Oranges attempt 1 and the Max Area breadth-first search rewrite.
- **`for...of` for values, `for...in` for keys.** `for...in` on an array yields string indices. Destructuring `[nx, ny]` from one gives garbage. Seen in Rotting Oranges attempt 1.
- **No comma operator in indexing.** `grid[nx, ny]` reads `grid[ny]`. Always write `grid[nx][ny]`. When you fix one instance, search the function for its siblings. Seen in Rotting Oranges attempts 1 and 2.
- **Loop condition uses the loop variable.** Write `for (let b = 0; b < lenY; b++)`, never `a < lenY`. Seen in Number of Islands attempt 2.
- **Every variable spelled as declared.** `yn`, not `xy`. Distrust copy-pasted lines and re-read each one. Seen in the Max Area breadth-first search rewrite and Number of Islands attempt 1.

## Traversal rules

- **Mark visited at entry for depth-first search, and on enqueue for breadth-first search.** For depth-first search, mark right after the guard and before recursing. For breadth-first search, mark the instant you push, including the seed cell. Unmarked means infinite loops or double counting. Seen in Number of Islands attempt 2 and the Max Area breadth-first search rewrite.
- **The visited marker must collide with nothing.** The mark value has to be distinguishable from every cell state you still need to process, and the guard must skip exactly the marked states. Marking rotted oranges as `0`, which already means empty, while the guard only skipped `0`, let rotten `2` cells sail through, get re-queued, and corrupt the fresh counter. Prefer the positive test: process a neighbor only if `grid[nx][ny] === 1`. Seen in Rotting Oranges attempt 3.
- **Adjacency matches the problem statement.** Use 4-directional adjacency unless the statement explicitly says diagonals count. Read that sentence first. Seen in Number of Islands attempt 1.

## Bookkeeping

- **The function returns its result.** Write `return count` at the end, not just compute it. Seen in Number of Islands attempt 2.
- **Every counter is updated where it changes.** If fresh oranges rot, `freshCount--` happens at the rotting line, not somewhere else, and not never. Seen in Rotting Oranges attempts 1 and 2.
- **Value types match the problem grid.** `'0'` and `'1'` strings versus `0` and `1` numbers. Check the constraints, then stay consistent. Seen in Number of Islands attempt 1.
- **No shared-mutation surprises.** `Array(n).fill(x)` puts one shared `x` in every slot, and `splice` mutates in place. Prefer `slice` or fresh arrays. Seen in Number of Islands attempt 1 and the isSubsequence problem.

## Design, before coding

- **Two-phase problems get two phases.** Scan and collect first, then process after. Never run the main algorithm inside the counting scan. Seen in Rotting Oranges attempt 2.
- **Write the plan as 3 to 5 comment lines first**, then write code under each one. If a step has no comment, it has no design.

## The 60-second re-read

After writing, before running, read every line top to bottom and ask what the line does and whether that is what you meant. Most items above are caught here.

## Past mistakes & points to improve

- Every item in this checklist is a bug actually shipped in the October 9 to 11 prep sessions. The checklist itself is the pre-submit ritual: run it before submitting any grid breadth-first search or depth-first search solution, and when a new bug appears in a session, add it here so the same mistake is caught before it ships again.

## Source

Prepared by Copper bot from Robin's interview practice sessions, September–November 2026. Ask Copper bot for more detail on anything in this note.
