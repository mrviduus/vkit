---
name: interview-pair
description: Pair-programming and live-coding interview habits and practice drills. Use when the user is preparing for or practicing a pair-programming or live-coding interview, wants a mock session in an unfamiliar codebase, or asks for feedback on how they explained their thinking while coding.
---

# Pair-Programming Interview

Interviewers grade communication and process as much as the final code: how you get oriented, what you ask, how you explain trade-offs, and how you react to hints.

## The 3 facts: say them before the first edit

Before changing any file, say out loud:
1. **Who calls this?** Find the callers and importers (grep). "This is used by X and Y, so changing the signature affects both."
2. **What changes?** The public functions/classes and behavior affected.
3. **What is the task, in my words?** Restate the requirement and confirm: "So the goal is ..., and out of scope is ..., right?"

Forcing these facts makes you investigate instead of guessing, and it shows the interviewer your reasoning.

## Getting oriented in an unfamiliar codebase (about 10 min)

1. Manifest and build: `package.json` / `Gemfile` / `build.gradle`; how to run the app and the tests.
2. Entry points and directory map, top 2 levels.
3. Follow one request or flow from input to output.
4. Conventions: naming, error handling, test style. Copy them.
5. Run the existing tests once before changing anything.

Narrate as you go: "I'm looking for where X is wired up ... found it in Y."

## While coding

- Start with the simplest working version, say so, then improve: "Brute force first, O(n²), then optimize."
- Before choosing, name the trade-off: "A hash map gives O(1) lookup at the cost of memory; fine here because ..."
- Small steps, run often. Write or extend a test for the behavior.
- When stuck, say what you've tried and what you're considering. Ask for a hint early instead of going silent.
- Treat a hint as collaboration: "Good point, that also covers the empty case."
- At the end: walk through edge cases (empty, one element, duplicates, large input), complexity, and what you'd do with more time.

## Mock session mode

When the user asks for practice:
1. Pick a realistic task in a small real repo (or the user's), matching the target company's stack if known.
2. Act as the interviewer: give the task, answer clarifying questions, give hints only when asked or after long silence.
3. Don't write the solution. Let the user drive.
4. After the session, give feedback in three parts: **communication** (3 facts, narration, trade-offs), **process** (orientation, tests, incremental steps), **code** (readability, correctness, edge cases). Give concrete quotes or moments, then 1-2 things to practice next.
