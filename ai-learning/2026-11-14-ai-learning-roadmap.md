# AI Learning Roadmap — From Full-Stack Engineering to Applied AI

**Recorded:** 2026-11-14  
**Practice period:** September–November 2026

## Previously discussed topics

- AI-assisted engineering: coding agents, review, refactoring, test generation, safe evaluation and adoption.
- Personal AI journal: summarize entries, identify recurring themes and surface actionable insights.
- Enterprise RAG/agents: private data, retrieval, tool permissions, constrained environments, latency and reliability (GC AI / North-style discussions).
- Data quality / evaluations: Snorkel AI coding-agent benchmarks, rubrics and expert review.
- WoW Classic Forever: passive gameplay-data recorder as the first milestone for imitation-learning experiments.
- AI career direction: connect production React/TypeScript, Kafka/Elasticsearch and data systems experience to AI infrastructure/backend engineering.

## Learning sequence

1. **Data foundation:** traces, timestamps, schemas, provenance, dataset quality and train/test leakage.
2. **Applied LLMs:** prompting vs retrieval vs fine-tuning, embeddings/vector search, structured outputs, tool calling, failure modes.
3. **Agents:** state/memory, explicit tool permissions, human approval, timeouts, retries and safe escalation.
4. **Evaluation:** representative test sets, task success, precision/recall, cost/latency, regression gates and human review.
5. **Imitation learning:** state/action trajectories, behavior cloning, distribution shift, supervised baselines, evaluation in controlled environments.
6. **Serving:** APIs, streaming, observability, privacy, secrets handling and rollback—the area that leverages existing engineering strengths.

## Past mistakes and feedback

- **Observed objective shift:** For the WoW project, the initial goal became **reliable passive recording**, not immediately training a model or deploying an agent.
- **Observed distinction:** AI enablement/annotation/review work and production AI engineering are different job tracks; evaluate actual building and ownership scope.
- **Learning gap, not a documented mistake:** No validated model/evaluation baseline was found in prior discussions; avoid claiming an agent is already learned or robust.

## Points to improve

1. Pick one small dataset and write its task definition, schema, baseline, metric and failure taxonomy.
2. For every AI demo, distinguish **AI-generated output** from **measured time savings or task success**.
3. Study RAG retrieval quality, evals and agent safety before building elaborate orchestration.
4. Record experiment decisions and failures in this vault, not just implementation steps.

## Source

- **Documentation assistant:** ChatGPT (OpenAI).
- **Source material:** Robin's prior ChatGPT interview-preparation and AI-learning discussions, consolidated into this study note; not a verbatim transcript.
- **Discussion reference:** Practice period: September–November 2026.
- **For follow-up:** Share [AI Learning Roadmap — From Full-Stack Engineering to Applied AI](https://github.com/szlike/personal-vault/blob/main/ai-learning/2026-11-14-ai-learning-roadmap.md) with ChatGPT and ask for a deeper explanation, practice question, or review of past mistakes. Original chat permalinks were not recorded.
