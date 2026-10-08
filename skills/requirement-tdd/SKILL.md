---
name: requirement-tdd
description: Requirement-first TDD for any bug fix or feature. Use BEFORE writing or delegating code — turn each finding or request into an owner-decided numbered rule, write one acceptance test named after it (red), then only the minimum code (green). Also covers where requirements are stored, how to delegate to agents, and how to stop endless code-review loops.
---

# Requirement-first TDD

TDD has two halves: **a failing test first**, and **only the code that test needs**. Teams (and agents)
usually keep the first and skip the second. Then a bug-fix PR grows "while I'm here" mechanisms — a
guard just in case, parity with another client, a retry, a cache — each with its own tests, none asked
for. Every review round finds bugs in that extra code, and the PR never converges.

## 1. Requirements first — discuss, then go

- Write each requirement as one sentence from the **user's** point of view
  ("a downloaded book shows in the Library while online").
- If it hides a product choice (when exactly? what if the user removed it? which model? which default?),
  **ask the owner before coding or delegating**: two options and a recommendation. An agent never
  picks product behaviour.
- Record the answer as a numbered rule before the first test.

## 2. Store it as a numbered rule

Keep rules where the team reads about the feature — a feature doc, e.g.
`docs/features/<feature>.md`, section **`## Rules (owner-decided)`**:

```
LIB-1  A download adds the book to the Library. (2026-10-08, QA run #7 finding 1)
       Test: src/lib/library.test.ts › "LIB-1: download adds the book to the Library"
```

- ID = short area prefix + number. Never reuse an ID; retire with a strike-through and a pointer to the
  replacement.
- Rule and test point to each other: a rule without a test drifts; a test without a rule gets deleted as
  "weird behaviour".
- Before changing behaviour, read the feature's rules first.
- If the repo already has a home for decisions (ADRs, a spec folder), use it and keep the same shape.

## 3. Red → green → stop

1. **One acceptance test per rule**, named with the ID: `it('LIB-1: …')` / `Test_LIB1_…`.
   Run it and watch it **fail for the right reason**.
2. **Minimum code** to make it pass. Nothing no rule asks for.
3. Refactor while green. Stop.

Every extra idea (parity, a defensive guard, a nicer flow) becomes its own backlog item — **never part
of this change**.

Legitimate exceptions to test-first (state them in the PR): a throwaway spike; layout/pixel behaviour
the test environment can't see (verify on a device or with a browser test right after); copy-only UI.

## 4. Code-review loops

- Review every change before merge and fix what it finds.
- **Fix exactly the finding.** Don't add mechanisms while fixing.
- If a second round finds bugs in something *you added*, first ask **"can this be deleted?"** — usually
  yes. If a change keeps needing rounds, rebuild it minimal from the main branch.
- Merge when a round finds **no correctness bugs**; cleanup-only findings go to the backlog.
- After an agent edits large docs (changelog, status, project instructions), diff their size against
  the main branch — whole-file rewrites silently drop most of a file.

## 5. Delegating to an agent

The prompt must contain: the rules (ID + text), "one acceptance test per rule named with the ID, seen
red first", "minimum code, nothing else", "edit docs with targeted replacements, never whole-file
writes", and the exact verification to run. Ask for the test names (red → green) in the report.

## Checklist

- [ ] Requirement in one user sentence; product choices asked and answered
- [ ] Rule written in the feature doc with an ID
- [ ] Acceptance test named `<ID>: …`, seen red
- [ ] Minimum code, green; extras logged as separate backlog items
- [ ] Reviewed; findings fixed without new machinery
- [ ] PR description lists the rule IDs and their tests
