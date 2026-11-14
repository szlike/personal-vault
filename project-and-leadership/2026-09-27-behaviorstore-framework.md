# behaviorStore Framework

**Recorded:** 2026-10-06

## The project

behaviorStore is an in-house state-management framework built in 2023 for a data-driven visualization and trading platform. It extends RxJS and reopens the Redux middleware. It was broadly adopted by other front-end teams.

The framework is standalone. It extends Redux and RxJS only, and it was inspired by redux-observable and MobX.

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
