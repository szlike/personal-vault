# JVM Memory, Garbage Collection, and Concurrency

**Recorded:** 2026-11-14  
**Original preparation:** 2026-09-11; revisited 2026-10-29

## JVM memory and GC

- **Stack:** per-thread call frames and local variables; deep/unbounded recursion can lead to `StackOverflowError`.
- **Heap:** shared object allocation; GC reclaims unreachable objects after tracing from roots.
- **Metaspace:** class metadata resides in native memory (modern HotSpot).
- **Memory leak in managed code:** unintentionally retained reachable objects, e.g. caches, static collections, listeners, thread locals.
- **OOM vs stack overflow:** different exhaustion mechanisms; diagnose heap occupancy, allocation rate, GC pause times and thread stacks.

## Race conditions and visibility

```java
int count = 0;
// count++ is read + add + write; two threads can lose updates.

AtomicInteger safeCount = new AtomicInteger();
safeCount.incrementAndGet();
```

- **`volatile`:** makes writes visible and restricts certain reorderings; **does not make `count++` atomic**.
- **`synchronized`:** mutual exclusion plus happens-before/visibility at monitor entry/exit; synchronizing on different locks does not protect the same invariant.
- **`AtomicInteger`:** atomic operations for one variable via CAS/atomic primitives; a multi-variable invariant may still require a transaction/lock.
- **Deadlock:** circular waiting with held resources; prevent with fixed lock order, timeouts or reducing lock scope.

## `synchronized` declaration trap

- A **constructor** cannot be declared `synchronized`.
- A **class** cannot be declared `synchronized`.
- An instance synchronized method locks `this`; a static synchronized method locks the `Class` object; they are different monitors.
- Synchronized blocks are valid inside constructors or methods.

## Spring context

Spring singleton beans can serve concurrent requests: do not store request-specific mutable state in instance fields without safe synchronization/design. Contrast this with many browser JavaScript patterns that appear sequential because execution is event-loop based.

## Past mistakes and feedback

- **Observed learning gap:** Concurrency/JVM basics were identified as areas needing recall and interview fluency.
- **Verified distinction:** We discussed the `synchronized` class/constructor question as **preparation**; there is no confirmed record that Fitch actually asked it.
- **Potential trap:** Treating `volatile` as a substitute for atomic compound operations.

## Points to improve

1. Draw a two-thread interleaving that loses an increment.
2. Compare `synchronized`, `Lock`, `volatile` and `AtomicInteger` for a counter vs a multi-field invariant.
3. Explain safe mutable state in Spring controllers/services.
4. Practice one JVM incident: growing heap, rising GC pauses, high CPU, and how you would investigate.

## Source

- **Documentation assistant:** ChatGPT (OpenAI).
- **Source material:** Robin's prior ChatGPT interview-preparation and AI-learning discussions, consolidated into this study note; not a verbatim transcript.
- **Discussion reference:** Original preparation: 2026-09-11; revisited 2026-10-29.
- **For follow-up:** Share [JVM Memory, Garbage Collection, and Concurrency](https://github.com/szlike/personal-vault/blob/main/technical-concepts/2026-11-14-jvm-memory-gc-and-concurrency.md) with ChatGPT and ask for a deeper explanation, practice question, or review of past mistakes. Original chat permalinks were not recorded.
