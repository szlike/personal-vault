# React Rendering, State Ownership, and JavaScript Fundamentals

**Recorded:** 2026-11-14  

## Source

**Prepared by:** Codex (OpenAI)

**Basis:** Consolidated interview-preparation material in this repository. Treat examples, metrics, and ownership as prompts to verify against your firsthand experience; return to Codex with the note title for expansion or correction.
**Original frontend interview discussion:** 2026-09-16

## React rendering

- **State update:** a component update schedules rendering of that component; children are normally evaluated unless memoization/bailouts apply. No universal rule says a child can change *parent-owned state* without the parent rendering.
- **Child updates global state:** components that subscribe/select affected values can re-render; a parent *without that subscription* need not re-render just because a child changes separate external state.
- **`React.memo`:** can skip a render when props compare equal; it does not block a component's own state or consumed context changes.
- **Controlled input:** React state/props determine the input value (`value` + `onChange`); uncontrolled input leaves value in DOM/ref.
- **List keys:** stable identity across renders; index keys are unsafe for reorder/insert/remove when per-item state matters.
- **Optimization:** profile first; split boundaries, use selective subscriptions, stable prop identities, virtualization and memo where justified.

## JavaScript equality traps (questions from Sep 16)

```js
0 == ''           // true: loose coercion
null == undefined // true: special abstract-equality case
NaN === NaN       // false
Object.is(NaN, NaN) // true
[] === []         // false: different object identities
```

`===` avoids most coercion surprises, though `NaN` and object identity remain important. Distinguish `Object.is`, `===` and shallow prop comparison.

## Java vs JavaScript (Fitch discussion)

Java: statically typed, JVM execution, true multi-threaded shared-memory workloads, common backend ecosystem. JavaScript: dynamic runtime, browser and Node ecosystems, event-loop concurrency model with async I/O. Both can run backend services; choose based on workload, team, ecosystem and operational requirements, not “one is always faster.”

## Past mistakes and feedback

- **Verified discussion:** Child/global state updates, controlled components, `React.memo`, list keys and equality/coercion were raised after an interview in September.
- **Coaching need:** Explain *why* a render occurs and whether props, state, context or subscription actually changed; avoid blanket “React re-renders everything” claims.
- **Potential trap:** `React.memo` is not a correctness solution and does not prevent updates from changed context or internal state.

## Points to improve

1. Draw a parent/child/store subscription scenario and predict which components render.
2. Explain why sorting a list with index keys can attach state to the wrong item.
3. Use your dashboard story to demonstrate an actual profiler-driven optimization.
4. Answer Java-vs-JavaScript in 45 seconds, anchored in a concrete service vs UI example.

## Source

- **Documentation assistant:** ChatGPT (OpenAI).
- **Source material:** Robin's prior ChatGPT interview-preparation and AI-learning discussions, consolidated into this study note; not a verbatim transcript.
- **Discussion reference:** Original frontend interview discussion: 2026-09-16.
- **For follow-up:** Share [React Rendering, State Ownership, and JavaScript Fundamentals](https://github.com/szlike/personal-vault/blob/main/technical-concepts/2026-11-14-react-state-rendering-and-javascript.md) with ChatGPT and ask for a deeper explanation, practice question, or review of past mistakes. Original chat permalinks were not recorded.
