# Technical Leadership, Conflict, Code Review, and Communication

**Recorded:** 2026-11-14  
**Practice period:** September–November 2026

## Focus

Senior/Staff interviews reward visible technical judgment, influence without authority, mentoring, and pragmatic disagreement—not only implementation skill.

## Interview answer structure

1. **Position:** “I recommended X because our most important constraint was Y.”
2. **Evidence:** Describe competing choices, measurements and stakeholders.
3. **Decision:** How you resolved disagreement, documented a trade-off and aligned the group.
4. **Follow-through:** Implementation, review, incident response, measurable outcome.

## Story prompts grounded in our discussions

- **Leading ~5 developers:** What standards, delegation, technical direction, review practices or mentoring did you *personally* provide? Do not assume a formal manager role if this was technical leadership.
- **Code review:** Ask about correctness, contracts, maintainability, tests, performance and security. Example: a state update should not force all independent dashboard widgets to re-render.
- **Technology disagreement:** Contrast a simpler synchronous implementation with async processing only when ordering, latency, retry or throughput warrant it.
- **Stakeholder disagreement:** Frame the problem as reliability / customer impact / delivery constraints; offer a staged alternative or measurable experiment rather than 'I said no.'
- **Mentoring:** Give one specific junior developer challenge, the guidance provided, and how ownership changed afterward. Exact real examples need to be filled in.

## Language for respectful pushback

- “I see the benefit of that approach. My concern is the failure mode when **X** happens.”
- “Can we align on the consistency and latency requirements before choosing the architecture?”
- “My recommendation is **A** for now, with **B** as the upgrade path if volume grows.”
- “I'd like to finish the high-level flow, then we can dive into partitions and retries.”
- “Could I take a moment to clarify the assumption before answering?”

## Past mistakes and feedback

- **Observed:** Answers can be technically rich but too long, reducing clarity and perceived seniority.
- **Observed coaching need:** More assertive but respectful English, especially for interruptions and technical pushback.
- **Observed:** No fully verified workplace conflict story was retrieved, so don't fabricate disagreement or results.
- **Potential trap:** “We built it” hides personal role; “I built everything” undermines collaboration. Specify ownership accurately.

## Points to improve

1. Rehearse **short answer → mechanism → trade-off → stop** for both BQ and technical follow-ups.
2. Time yourself: 90 seconds for first answer, 30 seconds for a follow-up, 3–5 minutes for deep dive.
3. Bring two code-review examples: a correctness bug and an architectural/performance trade-off.
4. Make the conflict story show **listening, evidence, decision, and relationship afterward**.
5. Practice a simulated interviewer pushing back twice; defend assumptions calmly and revise when evidence changes.

## Source

- **Documentation assistant:** ChatGPT (OpenAI).
- **Source material:** Robin's prior ChatGPT interview-preparation and AI-learning discussions, consolidated into this study note; not a verbatim transcript.
- **Discussion reference:** Practice period: September–November 2026.
- **For follow-up:** Share [Technical Leadership, Conflict, Code Review, and Communication](https://github.com/szlike/personal-vault/blob/main/behavioral-questions/2026-11-14-leadership-conflict-and-communication.md) with ChatGPT and ask for a deeper explanation, practice question, or review of past mistakes. Original chat permalinks were not recorded.
