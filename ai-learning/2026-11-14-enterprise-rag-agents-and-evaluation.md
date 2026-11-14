# Enterprise RAG, Agents, and AI Evaluation

**Recorded:** 2026-11-14  
**Practice period:** September–November 2026

## Previous discussion context

GC AI/North-style enterprise agents over sensitive data; AI coding agents and benchmark evaluation at Snorkel; enterprise AI enablement and responsible AI-assisted engineering; interest in AI product engineering using React/Python/backend experience. These were **role and learning discussions**, not verified systems you built.

## RAG system design

1. **Ingest:** parse documents, retain source IDs, timestamps and access-control metadata.
2. **Index:** chunk carefully, embed, and optionally use keyword + semantic hybrid retrieval.
3. **Retrieve:** authenticate user, apply document permissions **before** content enters prompt, rerank relevant chunks.
4. **Generate:** use grounded context and citations; represent uncertainty and no-answer cases.
5. **Evaluate:** retrieval recall, answer faithfulness, permissions, freshness, latency and cost.

## Agent architecture

An agent is more than a chat interface: it plans or chooses steps and invokes tools. Treat each tool as an API with least privilege, typed inputs, audit trails, idempotency and bounded retries. For irreversible or high-impact actions, require appropriate human approval. Separate prompt instructions from trusted authorization logic.

## Coding-agent evaluation

- **Task set:** representative issues from a fixed repo snapshot, with hidden regression tests.
- **Rubric:** correctness, requirements coverage, maintainability, test quality and prohibited behavior.
- **Process:** capture changes, run tests, review failure categories and compare against a baseline.
- **Metrics:** pass rate, cost per successful task, latency, human review burden and flaky-test rate.
- **Pitfalls:** benchmark leakage, judge bias, overfitting prompts and grading style instead of true correctness.

## Past mistakes and feedback

- **Observed distinction:** Snorkel expert-contributor work emphasized labeling, coding-agent evaluations, rubrics and code review; it should not be confused with ownership of a deployed AI service.
- **Observed positioning:** GC AI application answers should emphasize practical, secure, scalable agents rather than vague enthusiasm for an AI startup.
- **Knowledge gap:** Actual RAG architecture, production metrics and agent evals are not verified personal achievements; use this note as technical study material.

## Points to improve

1. Draw a RAG architecture and explain where access control is enforced, including stale/deleted documents.
2. Design 20 test queries with exact sources and known no-answer cases.
3. Compare keyword, vector and hybrid retrieval using measured relevance and latency.
4. Distinguish deterministic workflow from agent autonomy; explain permission and reliability trade-offs.
5. Practice a two-minute interview description and an operational failure scenario.

