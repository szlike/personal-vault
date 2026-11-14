# TypeScript Contracts Across Frontend and Backend

**Recorded:** 2026-11-14  
**Related application:** GC AI, 2026-10-26

## Question practiced

“How do types travel between your frontend and backend?” This is a runtime-contract question as much as a TypeScript question.

## Answer pattern

1. Design the API request/response contract (OpenAPI/JSON Schema or shared source types).
2. Generate or share TypeScript interfaces/types where appropriate, but recognize TS is erased at runtime.
3. Validate incoming JSON at system boundaries (e.g. schema-based validation); treat network data as untrusted.
4. Version or evolve contracts compatibly: optional fields, defaults, additive changes, client release skew.
5. Use tests/CI to check endpoint behavior and code-generated client types.

## Interview-ready example

> On a React/TypeScript frontend I want typed API clients so the request and response shapes are checked at build time. But network JSON has no TypeScript runtime types. I would keep an explicit API schema, generate or share the contract where practical, validate on the server and test backward compatibility. That becomes especially important when the backend is Java or Python and we cannot literally share TS source types.

This is a suggested explanation; the exact schema/codegen tools used on your projects were **not confirmed** in the retrieved discussion.

## Past mistakes and feedback

- **Observed:** You wanted the answer tied to real experience instead of generic boilerplate.
- **Technical trap:** Importing a TypeScript interface does **not** validate parsed JSON at runtime.
- **Technical trap:** Sharing types without contract versioning leaves independent deployments vulnerable to schema changes.

## Points to improve

1. Identify the exact API/validation approach used in one real Citi/BMO app.
2. Explain client/server release skew and how you avoid breaking older consumers.
3. Prepare a concrete invalid payload and how it gets handled.

## Source

- **Documentation assistant:** ChatGPT (OpenAI).
- **Source material:** Robin's prior ChatGPT interview-preparation and AI-learning discussions, consolidated into this study note; not a verbatim transcript.
- **Discussion reference:** Related application: GC AI, 2026-10-26.
- **For follow-up:** Share [TypeScript Contracts Across Frontend and Backend](https://github.com/szlike/personal-vault/blob/main/technical-concepts/2026-11-14-typescript-api-contracts.md) with ChatGPT and ask for a deeper explanation, practice question, or review of past mistakes. Original chat permalinks were not recorded.
