# STAR Story Bank — Ownership, Impact, Incidents, and Failure

**Recorded:** 2026-11-14  

## Source

**Prepared by:** Codex (OpenAI)

**Basis:** Consolidated interview-preparation material in this repository. Treat examples, metrics, and ownership as prompts to verify against your firsthand experience; return to Codex with the note title for expansion or correction.
**Practice period:** September–November 2026

## Pattern

Deliver each story in about two minutes: **Situation (15%) → Task (10%) → Action (55%) → Result/Learning (20%)**. Say the answer first and keep optional technical deep dives for follow-up.

## Candidate stories (outlines, not invented incident reports)

### Elasticsearch 7 → 8 migration — ownership and technical risk

- **Situation:** Existing Elasticsearch 7 environment required migration; large, multi-product trading-related data and compatibility risks.
- **Task:** Maintain data continuity and application behavior while moving to a new cluster.
- **Action prompts:** Why remote reindex? How were mappings/analyzers and clients checked? How did you plan validation, cutover, rollback and cross-team coordination? Specify **your actual ownership**.
- **Result:** Fill in verified downtime, document counts, reindex duration, defects and operational outcome. These were not recorded in our sessions.
- **Follow-ups:** Dual-read vs dual-write; alias swap; avoiding event loss during reindex; checksums/counts; recovery if cutover fails.

### Visualization dashboard — performance and customer value

- **Situation:** Multiple live chart/grid widgets share a configurable trader dashboard.
- **Task:** Keep independent widgets responsive as events and layouts change.
- **Action:** Explain the state split, React re-render isolation, memoization/stable props, subscriptions, lazy loading and measurements you **actually used**.
- **Result:** Prepare measured latency, render counts, user feedback, or adoption. Do not invent percentage improvements.
- **Follow-ups:** Why Redux/RxJS? How did you profile? How did you prevent stale data and subscriptions?

### RFQ ownership — ambiguity and stakeholder alignment

- **Situation/Task:** Clarify what the trading workflow needs and translate requirements into delivered functionality.
- **Action prompts:** Prioritization, API/data contracts, trade-offs, difficult stakeholder request, release decision and cross-team communication.
- **Result:** Prepare a real release/quality/business outcome.

### CI/CD or developer productivity — influence

- **Potential evidence:** A deployment automation solution was described as used by approximately 20 developers in earlier resume discussions; confirm your role and adoption before asserting it.
- **Action prompts:** Problem with old pipeline, design alternatives, rollout, documentation and feedback.
- **Result:** Time saved and failure reduction require verification.

### Failed system design interview — learning

- **Actual setback:** Nubank Top-K time-window system design remained unfinished after spending too long on clarifications/implementation and not drawing the full architecture.
- **Learning:** Timebox requirements, draw end-to-end data flow by minute 5–10, then deep dive; repeatedly practice ambiguous prompts aloud.
- **Important:** Use a workplace mistake story for “tell me about a mistake at work” unless interview failure is appropriate to the question.

## Past mistakes and feedback

- **Observed:** Starting with technologies rather than customer problem, architecture, your decision, and evidence of impact.
- **Observed:** Sometimes over-explaining and losing the answer's headline.
- **Missing evidence:** Specific incident timeline, severity, production metrics, and exact conflict resolution are **not documented**; do not treat prompts above as established events.

## Points to improve

1. Write **eight real stories**: impact, conflict, leadership, incident, ambiguity, migration, failure, mentoring.
2. Put one explicit decision and rejected alternative in every story.
3. For a technical story, make an architecture diagram and prepare two levels of depth (two minutes, ten minutes).
4. Gather verifiable before/after measures; use qualitative outcomes honestly when metrics are unavailable.
5. Practice anticipated probes: Why you? Who disagreed? What went wrong? What would you do differently?

## Source

- **Documentation assistant:** ChatGPT (OpenAI).
- **Source material:** Robin's prior ChatGPT interview-preparation and AI-learning discussions, consolidated into this study note; not a verbatim transcript.
- **Discussion reference:** Practice period: September–November 2026.
- **For follow-up:** Share [STAR Story Bank — Ownership, Impact, Incidents, and Failure](https://github.com/szlike/personal-vault/blob/main/behavioral-questions/2026-11-14-star-story-bank.md) with ChatGPT and ask for a deeper explanation, practice question, or review of past mistakes. Original chat permalinks were not recorded.
