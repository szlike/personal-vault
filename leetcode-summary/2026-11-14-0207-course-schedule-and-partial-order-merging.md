# 207/210. Course Schedule and Merging Partial Orders

**Recorded:** 2026-11-14  

## Source

**Prepared by:** Codex (OpenAI)

**Basis:** Consolidated interview-preparation material in this repository. Treat examples, metrics, and ownership as prompts to verify against your firsthand experience; return to Codex with the note title for expansion or correction.
**Original practice:** 2026-10-19 and 2026-10-25/26

## Pattern

Build a directed graph with edge `prerequisite → dependent`, compute indegrees, enqueue all zero-indegree vertices, then use **Kahn's topological sort**. If fewer than all vertices can be processed, a cycle exists.

```js
function canFinish(numCourses, prerequisites) {
  const edges = Array.from({length:numCourses}, () => []);
  const indegree = Array(numCourses).fill(0);
  for (const [course, pre] of prerequisites) {
    edges[pre].push(course);
    indegree[course]++;
  }
  const queue = [];
  for (let i=0; i<numCourses; i++) if (indegree[i] === 0) queue.push(i);
  let head = 0, seen = 0;
  while (head < queue.length) {
    const node = queue[head++]; seen++;
    for (const next of edges[node]) if (--indegree[next] === 0) queue.push(next);
  }
  return seen === numCourses;
}
```

## Merging the ordered color/size exercise

Input order constraints included `red→blue`, `XS→M→L`, and `XS→S→M→XL`. Build a graph **per dimension**, adding edges between adjacent observed values and deduplicating nodes. One valid size output is `XS→S→M→L→XL`. Other relative ordering can remain unspecified if the data lacks constraints.

This is a **partial-order merge**, not lexicographic sort. Use topological ordering and detect cycles/contradictions. If a **unique** total order is required, at every step there must be exactly one zero-indegree candidate; otherwise the order is ambiguous.

## Complexity

O(V+E) time/space (plus storing output).

## Past mistakes and feedback

- **Observed:** You asked to trace `queue[head++]` and how the adjacency graph changes as indegrees decrease.
- **Observed:** Confusion over whether unconstrained values such as `1,2,4` have a deterministic relative order; the graph may permit multiple valid orders.
- **Potential trap:** Reversing course/prerequisite edges breaks the intended semantics; LC 207's feasibility can hide errors that LC 210's actual order reveals.

## Points to improve

1. Write down each edge before coding, then compute indegrees by hand.
2. Trace `0→1, 0→4, 2→3, 1→3`, showing alternative valid orders.
3. Explain uniqueness vs existence; add cycle detection.
4. Practice LC 207, LC 210, then Alien Dictionary / merge exercise.

## Source

- **Documentation assistant:** ChatGPT (OpenAI).
- **Source material:** Robin's prior ChatGPT interview-preparation and AI-learning discussions, consolidated into this study note; not a verbatim transcript.
- **Discussion reference:** Original practice: 2026-10-19 and 2026-10-25/26.
- **For follow-up:** Share [207/210. Course Schedule and Merging Partial Orders](https://github.com/szlike/personal-vault/blob/main/leetcode-summary/2026-11-14-0207-course-schedule-and-partial-order-merging.md) with ChatGPT and ask for a deeper explanation, practice question, or review of past mistakes. Original chat permalinks were not recorded.
