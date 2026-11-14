# 695. Max Area of Island

**Recorded:** 2026-10-11

## Problem

Find the area of the largest island in a grid of land and water cells. Land cells connected up, down, left, or right form one island. LeetCode problem 695.

## Pattern

Depth-first search (DFS) flood fill. Scan every cell. When an unvisited land cell is found, flood fill from it and count the cells in that island, then keep the largest count seen.

## Attempts

The depth-first search version was correct on the first try. The breadth-first search (BFS) rewrite was broken by the `return` versus `continue` bug: the neighbor loop used `return` where it needed `continue`, so one bad neighbor abandoned the whole search. This is the same bug class as in LeetCode problem 994, Rotting Oranges.

## Complexity

- Time: each cell is visited once, so the time grows with the number of cells in the grid.
- Extra space: the recursion stack grows with the size of the largest island in the depth-first search version.

## Key insight

The area of an island is just a count collected during the flood fill. The traversal shape is the same as Number of Islands, with a counter added.

## Past mistakes & points to improve

- October 11, breadth-first search rewrite: `return` used instead of `continue` inside the neighbor loop. Fix: `return` exits the whole function, while `continue` skips only one neighbor. Re-check every early exit inside a neighbor loop, and remember this is the same bug class that appeared in Rotting Oranges, so it is a repeat, not a new mistake.
