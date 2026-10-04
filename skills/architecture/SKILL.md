---
name: architecture
description: Software architecture and system design - document an existing system with C4 Mermaid diagrams, design a new system or architectural solution step by step, record decisions as ADRs, and run system-design interview practice. Use when the user asks for architecture docs, architecture/C4/system/container/deployment diagrams, to design a system or feature architecture, to compare architectural options, "record this decision"/"ADR this", "why did we choose X", or system-design interview practice.
---

# Architecture

Four modes. Pick by request; they combine (e.g. design → diagrams → ADRs).

| Mode | Trigger | Output |
|---|---|---|
| **Document** | "document/diagram this system" | `docs/architecture/*.md` with C4 Mermaid |
| **Design** | "design X", "how should we build X", compare options | Design doc + diagrams + ADRs |
| **Decide** | a decision is being made, "ADR this", "why X?" | `docs/adr/NNNN-*.md` |
| **Practice** | "system design interview", "mock system design" | Interviewer-led session + feedback |

Ask before creating `docs/architecture/` or `docs/adr/` in a repo for the first time, and show drafts before writing ADRs.

## Mode 1: Document an existing system (C4)

1. **Explore before drawing.** Find what actually exists: manifests, entry points, deployables (each separately deployed/run thing is a *container*), data stores, queues, external APIs, config/infra files (Dockerfile, k8s, terraform, CI). Every element in a diagram must trace to code or config; don't invent.
2. **Pick levels by audience.** Context + Container are enough for most teams. Add Component only where it helps; Deployment for production systems; Dynamic for complex flows.

   | Level | Mermaid | Shows |
   |---|---|---|
   | 1 Context | `C4Context` | The system, its users, external systems |
   | 2 Container | `C4Container` | Apps, services, databases, queues |
   | 3 Component | `C4Component` | Inside one container |
   | Deployment | `C4Deployment` | Infra nodes |
   | Dynamic | `C4Dynamic` | Numbered request flow |

3. **Write one diagram per file** in `docs/architecture/`: `c4-context.md`, `c4-containers.md`, `c4-components-{feature}.md`, `c4-deployment.md`, `c4-dynamic-{flow}.md`. Each file: title, the diagram, then 3-8 lines explaining what matters (key flows, where state lives, risks). Add `docs/architecture/README.md` linking them.

**Diagram rules:**
- Every element: name, type, technology, short description (< 50 chars).
- One-way arrows labeled with verbs plus protocol: `Rel(api, db, "Reads orders from", "SQL")`. No bare "uses".
- At most about 20 elements per diagram; split otherwise. Always a title. Meaningful aliases (`orderService`, not `s1`).
- Container = deployable/runnable. A shared library is not a container. Show individual topics/queues, not one "Kafka" box.
- Microservices owned by one team are containers; owned by separate teams, they become systems.

Syntax: `references/c4-syntax.md`. Patterns (microservices, event-driven, deployment, API flows): `references/advanced-patterns.md`. Check the diagram against `references/common-mistakes.md` before finishing.

## Mode 2: Design a new system or solution

Work through these in order and show each step to the user; don't jump to a final design.

1. **Requirements.** Functional (what users do; the 3-5 core use cases) and non-functional (scale, latency, availability, consistency, security, cost). Ask about unknowns; state assumptions explicitly.
2. **Estimates.** Back-of-envelope: users, requests/sec (average and peak), read/write ratio, data size per year, bandwidth. Round numbers; show the math. They decide what matters (e.g. caching, sharding).
3. **API and data model.** Key endpoints/events and main entities with access patterns. Choose storage from the access patterns, not habit.
4. **High-level design.** C4 Context + Container diagrams. Walk one main request end to end.
5. **Deep dive on the riskiest 1-2 parts.** Bottleneck, hot path, or hardest consistency problem: caching, partitioning, queues, idempotency, failure handling.
6. **Trade-offs and alternatives.** For each major choice: what we gave up, what we rejected and why. Record significant ones as ADRs (Mode 3).
7. **Failure and operations.** What happens when each dependency is down or slow; monitoring, alerts, rollout/migration plan.
8. **Start simple.** Prefer the simplest design that meets the stated requirements (a monolith + one database is often right). Name the point at which it would need to change.

Output: `docs/architecture/design-{name}.md` with sections 1-7, diagrams inline, links to ADRs.

## Mode 3: Record decisions (ADR)

Suggest an ADR when a significant choice is made (framework, datastore, pattern, API style, auth, infra) or someone says "we decided..." / "X instead of Y because...". Not for trivial choices.

Location: `docs/adr/NNNN-kebab-title.md` (next number after the highest existing one) and an index row in `docs/adr/README.md`.

```markdown
# ADR-NNNN: <Decision title>

**Date**: YYYY-MM-DD
**Status**: proposed | accepted | deprecated | superseded by ADR-NNNN

## Context
<2-5 sentences: the problem, constraints, forces.>

## Decision
<1-3 sentences, present tense: "We use X.">

## Alternatives considered
- **<Option>**: pros / cons / why not.

## Consequences
- Positive: ...
- Negative: ...
- Risks: ... (and mitigation)
```

Index row: `| [NNNN](NNNN-title.md) | <Title> | <status> | <date> |`.

Rules: specific ("PostgreSQL", not "a database"), the *why* matters most, include rejected alternatives, readable in 2 minutes. Never edit an accepted ADR's decision; write a new one that supersedes it and update the old status. Backfilled decisions note the original date.

For "why did we choose X?": read `docs/adr/README.md`, then the matching ADR; if none, say so and offer to record one.

## Mode 4: System-design interview practice

1. Give a classic prompt suited to the user's level (URL shortener, rate limiter, news feed, chat, notification system, ride matching, e-commerce checkout, flash sale), or use the user's choice.
2. Act as the interviewer: answer clarifying questions with realistic numbers, push back on hand-waving ("what happens when that cache node dies?"), give hints only when stuck. Don't design it for them.
3. Follow Mode 2's steps as the expected structure; 35-45 minutes of content.
4. Feedback afterwards: **structure** (requirements before design, estimates used to make decisions), **depth** (deep dive quality, failure handling), **trade-offs** (stated alternatives and why), **communication**. Concrete moments, then 1-2 things to practice next. Offer a reference design as C4 diagrams.

## Rendering and sharing

- Mermaid in Markdown renders on GitHub; it's the default for repo docs.
- For a polished, shareable visual (presentation, review, stakeholder page), hand off to the `artifact-diagramming` skill if available, using the C4 model as the source of truth.
