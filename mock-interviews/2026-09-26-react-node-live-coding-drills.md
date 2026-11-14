# React and Node Live-Coding Drills

**Recorded:** 2026-09-27

## The drills

These were live-coding drills run on September 26 and 27, 2026, covering React and Node.js.

## ListingSearch component

The ListingSearch component needed a 300 millisecond debounce, an AbortController to cancel in-flight requests, loading, error, and empty states, and unmount safety so no state update happens after the component is gone.

The bugs that kept repeating across rewrites were:

- Inverted `Array.isArray` logic that survived rewrites unchanged, so the check kept failing the same way.
- A `submitSearch:` label syntax error.
- `this.$refs`, which is a Vue pattern, used inside React code.
- Uncontrolled inputs with no value binding.
- `click` used instead of `onClick`.
- A JSX (JavaScript XML) quote collision.

## Login form drill

The login form drill had its own repeated bugs:

- Inverted validation logic.
- `mount.current` used instead of `mounted.current`.
- `isValidated` was renamed, but the old references were left behind, which caused a ReferenceError on render. The grade for that attempt was D.
- A capital-R `React` import.

The final version graded B plus. One nit remained: stale prompts never clear.

## Past mistakes & points to improve

- After a rename, sweep every reference. The `isValidated` rename left old references behind and produced a ReferenceError on render, which is a bug that a single search would have caught.
- After a rewrite, re-check the exact bug that failed last time instead of assuming it is gone. The inverted `Array.isArray` logic survived rewrites unchanged, which means the rewrite was never tested against the last failure.
- Framework habits cross over silently. `this.$refs` is Vue, not React. When switching frameworks in a drill, name the framework out loud before writing the first line.
