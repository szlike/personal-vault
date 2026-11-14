# Interview Retrospectives — Nubank, Fitch, eBay, Pinterest

**Recorded:** 2026-11-14  
**Practice period:** September–November 2026

## Interview evidence

| Context | Known topics / results | Review priority |
| --- | --- | --- |
| Nubank | System design on Top-K access/visit counts over the last hour and historical windows; architecture incomplete under time pressure | Architecture-first response and timeboxing |
| Fitch | React/frontend, Java vs JavaScript, when to use JavaScript, technical direction, code review | Crisp frontend depth and real leadership examples |
| eBay MTS2 (earlier loop) | Completed four interviews; later reapplied and had a new recruiter screen in November; standardized proprietary codebase may repeat | Structured code explanation, honesty about prior exposure, strong architecture and BQ |
| Pinterest | Practiced merging partially ordered color/size sequences (a user-described interview-style exercise); topical review also included Topological Sort | Graph modeling and ambiguous relative order |
| Goldin/eBay preparation | Hiring manager, frontend/backend coding, system design, architect-style discussion were outlined in the repository | Correctness, traceability, high-value marketplace trade-offs |

**Evidence rule:** Do not equate coaching questions with actual interviewer questions. In particular, `synchronized` constructors/classes and many Spring concepts were **prep topics**, not verified questions asked by Fitch.

## Past mistakes and feedback

- Nubank: requirements and low-level details consumed too much of the allotted design time; complete graph/dataflow came too late.
- Coding: recurrent JavaScript correctness traps appeared—base cases, boundaries, `node.val`, queue indexing, comparators, duplicate work.
- Communication: many accurate mechanisms are less persuasive without concise framing and the business/system-level implication.
- eBay repeat assessment: prior exposure to a proprietary codebase could make familiarity seem misleading. If the same exact exercise appears, briefly disclose having previously seen it and allow the interviewer to direct the assessment.

## Points to improve — next practice loop

1. **One system design (45 minutes):** requirements 5, high-level diagram 5, data model/API 8, scale/failure 15, deep dive/trade-offs 10, recap 2.
2. **Two coding problems (40 minutes each):** clarifications, baseline approach, optimal algorithm, tests, complexity and verbal trace.
3. **Two behavioral stories:** recording and feedback on concision, evidence, personal contribution and follow-ups.
4. **One project deep dive:** draw the dashboard/ES migration architecture from memory and answer operational failure questions.
5. Record each new mistake with date, reproduction input, root cause, corrected invariant and next review date.

## Source

- **Documentation assistant:** ChatGPT (OpenAI).
- **Source material:** Robin's prior ChatGPT interview-preparation and AI-learning discussions, consolidated into this study note; not a verbatim transcript.
- **Discussion reference:** Practice period: September–November 2026.
- **For follow-up:** Share [Interview Retrospectives — Nubank, Fitch, eBay, Pinterest](https://github.com/szlike/personal-vault/blob/main/behavioral-questions/2026-11-14-interview-retrospectives-and-practice-plan.md) with ChatGPT and ask for a deeper explanation, practice question, or review of past mistakes. Original chat permalinks were not recorded.
