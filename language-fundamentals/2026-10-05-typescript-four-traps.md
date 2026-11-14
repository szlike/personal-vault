# TypeScript Four Traps

**Recorded:** 2026-10-06

## Context

TypeScript fundamentals drill, October 5 to 6, 2026. Four traps, each corrected against the TypeScript compiler version 5.5 in strict mode. A cheat sheet was saved after the drill.

## Trap 1: `??` versus `||` on optional undefined

The `??` operator (nullish coalescing) falls back only when the left side is null or undefined. The `||` operator falls back on any falsy value, which includes `0`, the empty string, and `false`. When a value is optional and may legitimately be `0` or an empty string, use `??` so a real value is not replaced by the fallback.

## Trap 2: `unknown` forbids all property access

The `unknown` type forbids all property access, including access with the optional chaining operator `?.`. A value typed `unknown` must be narrowed first, for example with a type guard or a `typeof` check, before any property can be read from it.

## Trap 3: `as`-cast is assertion, not validation

An `as` cast in TypeScript is an assertion, which is a claim to the compiler, not validation at runtime. It does not check the value. If the value is actually a different shape, the cast hides the problem and the code fails later at runtime. Real validation means checking the value with code that runs.

## Trap 4: Closure narrowing survives unless a later reassignment kills it

When a variable is narrowed inside a closure (a function that captures a variable from its surrounding scope), the narrowing survives unless a later reassignment of that variable removes it. The assistant initially gave the wrong answer on this trap and corrected it after Robin pushed back with "that doesn't sound right, prove it". The correction was verified against the compiler.

## Past mistakes & points to improve

- None outstanding. All four traps were closed with compiler evidence, and the cheat sheet was saved.
- Keep the habit that closed this drill: when an answer feels wrong, ask for proof from the compiler instead of accepting it.

## Source

Prepared by Copper bot from Robin's interview practice sessions, September–November 2026. Ask Copper bot for more detail on anything in this note.
