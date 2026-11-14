# behaviorStore Stress Test (Gleb Prep)

**Recorded:** 2026-10-06

## Context

behaviorStore stress test, Gleb prep, Oct 4-6. Five questions Gleb would ask about his 2023 in-house RxJS/Redux framework. behaviorStore is the state-management framework he built for a data-driven visualization and trading platform, exported as the Epic class.

## The sideEffect backdoor

Epics could return a custom sideEffect function for logging, debugging, and analytics only, never for touching state. The backdoor is enforced by convention, not construction: nothing in the code stops a sideEffect from touching state. It is a rule the team agreed to follow.

Proposed fix: narrow the signature so the sideEffect only receives logging and analytics interfaces, never the store or dispatch. If the function cannot reach the store, it cannot touch state, and the convention becomes a construction.

## Weak spot

He dodged the enforcement question twice in the mock. He could describe the backdoor but could not defend how it is prevented. Describing a hole is not the same as defending it, and the dodge was visible.

## Status

OPEN. Still the weakest Architecture answer.

## Past mistakes & points to improve

- Own the convention-not-construction answer directly instead of dodging. Say: "Enforced by convention. Here is the failure mode, and here is the signature change that would close it." A direct admission with a concrete fix is stronger than an evasion.
- Rehearse the exact sentence before the next Architecture round, because the enforcement question is the predictable follow-up to any backdoor admission.
- Do not dodge a question twice. The first dodge is a hesitation; the second dodge becomes the story of the round.

## Source

Prepared by Copper bot from Robin's interview practice sessions, September–November 2026. Ask Copper bot for more detail on anything in this note.
