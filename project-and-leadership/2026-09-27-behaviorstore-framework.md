# behaviorStore Framework

**Recorded:** 2026-10-06

## The project

behaviorStore is an in-house state-management framework built in 2023 for a data-driven visualization and trading platform. It extends RxJS and reopens the Redux middleware. It was broadly adopted by other front-end teams.

The framework is standalone. It extends Redux and RxJS only, and it was inspired by redux-observable and MobX.

## Final interview answer

**Interview question:** Tell me about a recent project you’re proud of. What problem were you solving, what was your specific role, and what was the outcome?

I led the development of an internal state management library that several other teams later adopted.

We are talking about a UI application and as our application grew, tracing a single button click meant jumping across files to follow API calls, data transformations, and UI updates. We also wanted a consistent way to see which features people used, how long they took, and where they failed.

I built the library with Redux and RxJS, inspired by Redux Observable. We gave each user workflow a clear entry point and we draw the boundary on where the workflow finish. This library also provide a centrialized  pipeline with steps that engineers could follow. That also gave us a standard place to handle errors and record performance.

The library also gave teams a clear convention for handling side effects, mitigate the risk of misuse.

Several teams adopted the library, and we were able to send usage, timing, and error data to our central tracking platform.

## Interview follow-up on adoption

**Interview question:** Let’s talk about how BehaviorStore caught on. Pick one team that adopted it. What did they need before they were willing to try it, and what did you personally do to help them get started?

We first rolled out BehaviorStore across several UI repositories on my team. I wrote an onboarding guide and ran demos within our department. I used feedback from those demos to make the library easier to use, then presented it at our organization-wide Innovation Day.

I designed it to be easy to try. Teams could add a single middleware to their existing Redux store and use a configuration flag to choose whether to follow the stricter RxJS pipeline conventions or keep their existing Redux patterns.

Usage tracking drew the most interest. Every UI project needed to send usage data to our central platform, and the library gave teams a consistent way to capture it. Each project could use the built-in tracking instead of building its own. Several engineers reached out with questions after the demo.

## How it works

The framework exports an Epic class. An epic has the signature (action$, state$) => action$, which means it takes a stream of actions and a stream of state, and returns a stream of actions. Each epic carries a description field, and epics are registered in a CONSTANT registry. Timers run around dispatch so the cost of each action is visible.

Mid-pipeline API (Application Programming Interface) calls were fetched and merged with mergeMap and forkJoin, and they always ended in a dispatched action. That is normal pipeline work, not a side effect.

The sideEffect hook corresponds to tap, which is the RxJS operator for fire-and-forget effects. It was for logging, debugging, and analytics only, and it never touched state. Dispatching inside tap is the anti-pattern the backdoor governed.

## What was planned but never built

Error replay was a planned purpose for the framework. It was never built. It must never be presented as a built feature.

## Enforcement: convention, not construction

The sideEffect hook was enforced by convention, not by construction. Nothing in the code prevented an epic from returning a sideEffect function that touched state. Robin calls this "a backdoor to this framework."

His framing for the project is: "you didn't invent epics; you made epics adoptable."

## Status

OPEN. The enforcement question is the open weak spot in this story.

## Past mistakes & points to improve

- The enforcement question was dodged twice in the October 4 to 6 mock sessions. Do not dodge it again. Own the convention-not-construction answer directly: the rule existed as a team convention, the code did not enforce it, and that was a known trade-off. A direct answer about a real weakness is stronger than a dodge that invites a third question.
- Keep the built versus planned line sharp. Error replay was planned, never built. Say that in the first sentence whenever the framework's purposes are listed.

## Source

Prepared by Copper bot from Robin's interview practice sessions, September–November 2026. Ask Copper bot for more detail on anything in this note.

The final interview answer comes from Robin’s eBay hiring manager interview practice on November 14, 2026. Codex helped polish the wording. Ask Codex for more detail about this answer.

Codex polished the adoption follow-up from Robin’s eBay hiring manager interview practice.
