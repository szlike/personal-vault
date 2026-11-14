# Scooter Street Simulation (Reconstructed)

**Recorded:** 2026-10-15

**Status:** OPEN

## Problem

This is a HackToHire question, reconstructed from the HackToHire site. He extracted the question from the site's Next.js data payload, and the assistant decoded it and reconstructed the problem statement. It is tagged as Pinterest, was asked 6 times, and appears at the online-assessment stage. The tagged topics are Sorting and Simulation.

## The flaw he caught

The first reconstruction framed the objective as minimum riding distance. He caught the flaw himself: if the objective were minimum riding distance, the answer is always 0, because you could walk everything. The problem was reframed as minimum walking, which is the same as maximum riding, and that is a real problem.

## Past mistakes & points to improve

- Provenance discipline: reconstructed material must be marked as reconstructed in the same message, honestly, and kept separate from sourced material. He correctly demanded "did you make up anything?" and got an honest accounting of what was reconstructed and what was sourced. Keep doing that check on every reconstructed question.
- No coded solution was found in the transcript, so this problem is still open. The next step is to code a solution against the reframed objective and paste the attempt.
