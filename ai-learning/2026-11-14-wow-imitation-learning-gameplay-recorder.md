# WoW Imitation Learning — Gameplay Recorder and Dataset Design

**Recorded:** 2026-11-14  

## Source

**Prepared by:** Codex (OpenAI)

**Basis:** Consolidated interview-preparation material in this repository. Treat examples, metrics, and ownership as prompts to verify against your firsthand experience; return to Codex with the note title for expansion or correction.
**Original discussion:** 2026-11-11

## Goal and scope

Longer-term idea: learn navigation/travel behavior in WoW Classic Forever (walking, zeppelins, flight paths), then possibly explore simple grinding and recovery routines. You explicitly preferred a **learned behavior model** over a rules-only bot.

**First milestone:** A passive, reliable gameplay recorder and viewer/converter for future supervised learning. Do **not** start with autonomous control or model training. The game AddOn can write an offline SavedVariables file; a real-time backend is not required.

## Previously discussed data sources

| Source | Candidate fields | Limits |
| --- | --- | --- |
| Lua AddOn / SavedVariables | map ID, normalized X/Y, facing when available, HP, target, timestamps, game events | AddOn APIs expose only allowed in-game state; cannot freely write arbitrary live files |
| External/manual input recording | WASD, space, mouse buttons/motion, key-down/up and durations | Clock alignment, focus, input interpretation |
| Screenshot/video samples | frames, UI/game context, visual landmarks | Storage rate, privacy, frame timing, visual occlusion |
| Session metadata | objective, destination, zone, result, recording version | Labels must be trustworthy |

## Suggested dataset schema

```json
{
  "session_id": "example-session",
  "timestamp_ms": 0,
  "observation": {"map_id": null, "x": null, "y": null, "hp_pct": null},
  "action": {"key": "W", "state": "down", "duration_ms": 120},
  "goal": {"type": "travel", "destination": null},
  "next_observation_ref": null,
  "source": "manual-recording",
  "quality_flags": []
}
```

This is a proposed schema, **not** a claim that the recorder exists. Store raw events separately from derived training windows; include schema version, clock synchronization and dropped-event counters.

## Learning design

1. Record sessions with clearly labeled start/goal/outcome; retain failures, not just successes.
2. Align sampled observations with action start/end times; test the alignment on a short known path.
3. Create route/trajectory features, split **by session/route** rather than random adjacent frames to avoid leakage.
4. Start with a simple supervised baseline in an offline replay or controlled test setting.
5. Measure goal completion, deviation, time-to-destination, stuck loops and behavior under novel views.

## Past mistakes and feedback

- **Observed scope risk:** Discussed many components (HDMI capture, OpenCV, AddOn telemetry, model, web viewer, automation) before settling on a recorder-first milestone.
- **Observed constraint:** Accessing game memory or automating gameplay on official servers risks account bans and may breach game rules. Do not treat external capture/input hardware as permission to bypass those rules.
- **Potential ML traps:** Treating one frame as complete state, failing to sync clocks, collecting only successful demonstrations, or mixing near-identical training/test trajectories.

## Points to improve

1. Implement and validate a small end-to-end recording session before expanding field count.
2. Define `observation → action → next observation` and timestamps precisely.
3. Track dropped data, unknown fields, versioning and objective labels.
4. Separate data science experiments from deployment; check applicable game rules before any automation.
5. Keep a web viewer lightweight: inspect a trajectory, timestamps, actions and visual context.

## Source

- **Documentation assistant:** ChatGPT (OpenAI).
- **Source material:** Robin's prior ChatGPT interview-preparation and AI-learning discussions, consolidated into this study note; not a verbatim transcript.
- **Discussion reference:** Original discussion: 2026-11-11.
- **For follow-up:** Share [WoW Imitation Learning — Gameplay Recorder and Dataset Design](https://github.com/szlike/personal-vault/blob/main/ai-learning/2026-11-14-wow-imitation-learning-gameplay-recorder.md) with ChatGPT and ask for a deeper explanation, practice question, or review of past mistakes. Original chat permalinks were not recorded.
