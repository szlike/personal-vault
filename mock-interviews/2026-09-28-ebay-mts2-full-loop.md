# eBay MTS 2 Full Loop

**Recorded:** 2026-10-14

## The loop

MTS means Member of Technical Staff. This was the eBay MTS 2 interview loop, plus one extra round added afterwards.

The screening round came first, with Eric Biewener, Principal Software Engineer.

Round 1 was Node.js and Web Fundamentals with Sree Sai Chaitanya Nagarajugari. There were technical issues, and deep Express follow-ups consumed the remaining time. The round went well.

Round 2 was Team Fit with Arun Selvaraj, the hiring manager. See the Team Fit note for the detail: Arun asked twice whether Robin leans frontend or backend, and Robin leaned on live answers instead of his prepared stories.

The System Design round was with Usha Achladi, on a seller dashboard.

The Architecture round was with Anton Ganeshalingam, on seller inventory management.

An extra round was added on October 6 with Gleb Ryshkov, Senior MTS and Architect. Robin self-reported that it went badly. He was caught on WebSocket, on express-ws, on res.send, res.write, and res.end, and on CommonJS versus ECMAScript Modules (CJS versus ESM).

## Decision

Rejection on October 14, 2026, by Workday template email. The thread is closed.

## Preparation structure

The preparation followed a 5-day battle plan from September 23. Day 1 was system design. Day 2 was distributed systems. Day 3 was Java concurrency, JVM (Java Virtual Machine), and DSA (Data Structures and Algorithms). Day 4 was a full mock loop. Day 5 was sharpen, do not cram. The plan was compressed once the real calendar arrived.

## Compensation research

Levels.fyi put MTS 2 at about CA$239,000 total compensation. MTS 1 was about CA$218,000 total, made of CA$165,000 base, CA$35,000 stock, and CA$17,900 bonus. Sign-on pay is not structural at eBay, but it is negotiable as make-whole compensation for what a candidate gives up by leaving.

## Past mistakes & points to improve

- The per-round notes live in their own files (Team Fit, seller dashboard, inventory management, Gleb fallout). Cross-reference them rather than repeating them here.
- The loop's lesson is that the extra round exposed the Node.js honesty gap. Daily-driver claims must match reality. The Node.js worker-pool service was a proof of concept, never productionized, and the depth in the Sree round came from preparation drills, not daily production work. Frame it as preparation-drill depth, not as a daily driver.
