# Node.js / TypeScript Rapid-Fire

**Recorded:** 2026-10-06

## Context

Rapid-fire quiz session, October 4 to 6, 2026, as preparation for the extra interview round with Gleb Ryshkov (eBay Senior Member of Technical Staff / Architect, October 6). The quiz covered the Node.js event loop, file input and output, the `unknown` type, worker pools, `await` and Promise types, and Promise.all behavior.

## Question 1: Event loop order

The question asked for the execution order of a small snippet mixing synchronous code, microtasks, and timers. The correct order was a, d, c, b. Robin got it right on retry, after getting it wrong in the September session.

## Question 2: `readFileSync` blocks the single thread

`fs.readFileSync` blocks the one Node.js thread while the file is read, so nothing else runs during that time. Robin first said "add async", which was corrected to the concrete fix: use `fs.promises.readFile`. A worker pool is the wrong tool for input and output work. A worker pool is the right tool for CPU-bound work, such as D3 plotting done on the server.

## Question 3: `unknown` forbids all property access

The `unknown` type in TypeScript forbids all property access, including access with the optional chaining operator `?.`. You must narrow the value first, for example with a type guard, before you can touch any property on it.

## Question 4: Worker-pool proof of concept critique

Robin built a worker-pool service as a proof of concept (a small trial built to test an idea, never put into production). The critique of that design: creating a new worker per request defeats the point of having a pool; there was no backpressure, meaning nothing slowed down new work when the pool was full; and workers should be capped at the core count of the machine. The session also compared the cluster module with worker_threads: cluster starts a separate process per core, while worker_threads run threads inside one process.

## Question 5: `await` unwraps `Promise<T>` to `T`

`await` on a `Promise<T>` gives back a value of type `T`. Robin misread the type `{name: string}` as the type string. It is an object type with one property, `name`, whose type is string.

## Question 6: Promise.all is fail-fast

`Promise.all` rejects as soon as any one of the promises rejects, and it does not cancel the others. The other promises keep running to completion in the background; their results are simply ignored.

## Note on quiz format

In rapid-fire quizzes, Robin wants each question to carry its full code context inline, instead of requiring him to scroll back up to see the whole question.

## Past mistakes & points to improve

- Read the type literally. `{name: string}` is an object type, not the string type. When a type looks short, say out loud what each part of it means before answering.
- Input and output blocking is fixed by asynchronous input and output, not by a worker pool. When a question is about blocking, first ask whether the work is input and output bound or CPU bound, then pick the tool.
- Worker pools need a fixed size capped at the core count, reuse of workers across requests, and backpressure when the pool is full. Creating a worker per request is not a pool.
