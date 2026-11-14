# 130. Surrounded Regions

**Recorded:** 2026-10-12

## Problem

Given a grid of `X` and `O` cells, flip every `O` region that is fully surrounded by `X` into `X`. An `O` region that touches the border is not surrounded and must stay `O`. LeetCode problem 130.

## Pattern

Invert the hunt. Instead of searching from inside the grid for surrounded regions, flood fill from the borders. Mark every `O` that is reachable from a border cell as safe, because those regions are not surrounded. Then flip every unmarked `O` to `X`, since only surrounded regions remain.

## Attempts

Solved correctly on his own. The solution was verified against a reference on 500 random boards plus edge cases. An optimized version was measured at 8.4 milliseconds down to 2.6 milliseconds, about 3 times faster.

## Complexity

- Time: each cell is visited a constant number of times, so the time grows with the number of cells in the grid.
- Extra space: the recursion stack or the queue grows with the number of border-connected cells in the worst case.

## Key insight

The border is the starting point, not the inside. A region is safe exactly when it touches the border, so flood filling outward from the borders finds every safe cell in one pass. Whatever is left over after that pass is surrounded by definition.

## Past mistakes & points to improve

- None outstanding. This is banked as a clean solve. The inversion pattern is the takeaway: when a problem asks which regions are enclosed, flood fill from the outside in and flip what remains.

## Source

Prepared by Copper bot from Robin's interview practice sessions, September–November 2026. Ask Copper bot for more detail on anything in this note.
