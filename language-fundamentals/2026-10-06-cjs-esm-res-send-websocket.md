# CommonJS vs ESM, `res.send` / `res.write` / `res.end`, WebSocket

**Recorded:** 2026-10-07

## Context

Session on October 6 to 7, 2026, following the extra eBay interview round with Gleb Ryshkov (Senior Member of Technical Staff / Architect). Gleb caught gaps in WebSocket knowledge, express-ws syntax, the difference between `res.send`, `res.write`, and `res.end`, CommonJS (CJS) versus ECMAScript Modules (ESM), and how a server knows the client received a response.

## CommonJS versus ECMAScript Modules

CommonJS, usually shortened to CJS, is the module system Node.js started with. It uses `require` to load a module and `module.exports` to expose values, and it loads modules synchronously. ECMAScript Modules, usually shortened to ESM, is the standard module system of JavaScript. It uses `import` and `export` statements, and it loads modules asynchronously. A Node.js project picks one system per file, and mixing them without care causes errors.

## `res.send` versus `res.write` versus `res.end`

In Express, `res.send` is a one-shot response: it writes the body, sets headers, and ends the response in a single call. `res.write` writes one chunk of the body and keeps the response open, so it is used for streaming, where data is sent in pieces over time. `res.end` finishes the response and closes it. For a streaming response, the pattern is one or more `res.write` calls followed by a final `res.end`.

## WebSocket

A WebSocket is a long-lived two-way connection between a client and a server, unlike HTTP (Hypertext Transfer Protocol), where the client asks and the server answers and the exchange ends. After the initial handshake, either side can send messages at any time. The express-ws library adds WebSocket support to an Express app, letting a route handle WebSocket connections alongside normal HTTP (Hypertext Transfer Protocol) routes.

## How the server knows the client received the response

HTTP (Hypertext Transfer Protocol) has no delivery receipts. The server sends the response and does not get a confirmation that the client received it. What exists instead is conditional requests: the server can send an ETag, which is a version tag for a resource, and the client can send it back later with the If-None-Match header, so the server knows whether the client already has the current version.

## October 7 reflection

Robin reflected on October 7: "I did it really wrote api using nodejs day to day. Not knowing Cjs and esm is fine." The assistant corrected the framing. Dismissing the gap is the wrong carry-forward. The honest framing is the right one: "I am not a daily Node.js API author", stated plainly, without pretending the gap does not matter.

## October 8: CodeSignal project task

On October 8, the CodeSignal project task "Milky Way Messenger" was reviewed. Feature 6, streaming, is `res.write` and `res.end`. Feature 1b is Express error handling. The task was framed as redemption material for the Gleb round, because it exercises exactly the streaming and Express knowledge that the round exposed.

## Past mistakes & points to improve

- Know the streaming trio cold: `res.write` sends chunks, `res.end` finishes the response, `res.send` is one-shot. Say the three out loud with their roles before answering any streaming question.
- Never dismiss a gap that an interviewer caught. Reframe it honestly instead: state plainly what your daily work is and is not, and do not claim the gap does not matter.
- For module systems, be able to say in one sentence each how CJS (CommonJS) and ESM (ECMAScript Modules) load and export, and why mixing them breaks.

## Source

Prepared by Copper bot from Robin's interview practice sessions, September–November 2026. Ask Copper bot for more detail on anything in this note.
