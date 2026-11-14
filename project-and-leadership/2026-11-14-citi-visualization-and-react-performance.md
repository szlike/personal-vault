# Citi Visualization Platform and React Performance Deep Dive

**Recorded:** 2026-11-14  

## Source

**Prepared by:** Codex (OpenAI)

**Basis:** Consolidated interview-preparation material in this repository. Treat examples, metrics, and ownership as prompts to verify against your firsthand experience; return to Codex with the note title for expansion or correction.
**Practice period:** September–November 2026

## Problem and scope

Internal trader-facing financial analytics/dashboard platform with configurable widgets for line/pie charts and grids. Technologies discussed: React, TypeScript/JavaScript, D3, Highcharts, AgGrid, RxJS/Redux, WebSockets, Kafka, Elasticsearch, drag/drop/resize layouts, server-side rendering or export use cases.

The product serves a comparatively small set of internal users but supports business workflows. Earlier discussion described **100K+ messages per product** and **10+ products**; confirm the metric's time unit and distinction between indexed documents, messages and requests before repeating it.

## Architecture narrative (adapt to actual implementation)

1. Data arrives from trading/vendor systems, is transformed and indexed for query/aggregation.
2. Backend APIs and subscriptions retrieve historical and real-time data.
3. Dashboard shell owns layout and widget identity; widgets own render-intensive chart/grid views.
4. Components subscribe selectively and avoid unrelated re-renders.
5. Export/share/email and embedded visualization increase operational surface area.

## Key technical deep dives

- **State isolation:** dashboard layout vs widget data vs transient UI state; why a global update can trigger renders and when memoization helps.
- **React performance:** use stable props, `React.memo`, selector/subscription granularity, virtualization for grids, and profiler measurements where actually implemented.
- **Live feeds:** reconnection, event ordering, late updates, throttling/coalescing and stale subscription cleanup.
- **Search/aggregation:** match UI needs to Elasticsearch query/index patterns; discuss refresh and eventual visibility.
- **Scale:** fan-out to widgets, burst updates, WebSocket backpressure and backend load—not just page-load time.

## Past mistakes and feedback

- **Observed coaching:** Frontend internals tended to dominate the story; explicitly state end-to-end data flow and business relevance.
- **Observed framing issue:** “Few users” was perceived as weak scale. Frame the complexity honestly: data volume, financial correctness, number of widgets/products, update frequency and operational criticality.
- **Gap:** No verified p95 latency, FPS, render count, usage or revenue attribution in our retrieved history; do not imply a quantified win.

## Points to improve

1. Draw a five-box architecture and name the team-owned service boundaries.
2. Prepare one before/after React Profiler trace and be able to discuss why the re-render happened.
3. Explain **why** RxJS, Redux and WebSockets were appropriate or what their costs were.
4. State two trade-offs: real-time consistency vs freshness; shared state simplicity vs widget isolation.
5. Bring one real production bug and the instrumentation that exposed it.

## Source

- **Documentation assistant:** ChatGPT (OpenAI).
- **Source material:** Robin's prior ChatGPT interview-preparation and AI-learning discussions, consolidated into this study note; not a verbatim transcript.
- **Discussion reference:** Practice period: September–November 2026.
- **For follow-up:** Share [Citi Visualization Platform and React Performance Deep Dive](https://github.com/szlike/personal-vault/blob/main/project-and-leadership/2026-11-14-citi-visualization-and-react-performance.md) with ChatGPT and ask for a deeper explanation, practice question, or review of past mistakes. Original chat permalinks were not recorded.
