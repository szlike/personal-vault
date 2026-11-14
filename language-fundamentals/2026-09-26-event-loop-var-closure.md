# Event Loop and `var` Closure

**Recorded:** 2026-09-27

## Context

Event loop drill plus the `var` closure bug, September 26 to 27, 2026. Saved as event-loop-summary.md.

## The event loop

JavaScript runs on one thread with an event loop. Tasks are split into macrotasks (such as timers and input and output callbacks) and microtasks (such as promise callbacks). After each macrotask finishes, the engine drains the whole microtask queue before it moves on to the next macrotask. Microtasks always run before the next macrotask, no matter how many microtasks are queued.

## The `var` closure bug

A loop that uses `var` and pushes a function into an array gives `3, 3, 3` when the functions are called later, not `2, 1, 0`. The reason is that `var` creates one single binding shared by every iteration of the loop, so all three functions read the same variable after the loop has finished. The fix is to use `let`, which creates a fresh binding per iteration, or to capture the value explicitly, for example by passing it into a function.

## Past mistakes & points to improve

- `var` in a loop shares one binding across all iterations. Use `let` for loop variables, or capture the value explicitly, so each iteration keeps its own copy.
- When reasoning about execution order, first separate microtasks from macrotasks, then remember that the microtask queue drains fully before the next macrotask starts.

## Source

Prepared by Copper bot from Robin's interview practice sessions, September–November 2026. Ask Copper bot for more detail on anything in this note.
