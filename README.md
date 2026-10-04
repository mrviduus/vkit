# vkit

Personal Claude Code plugin: skills and agents I actually use.

## Install

```
/plugin marketplace add mrviduus/vkit
/plugin install vkit@vkit
```

## Contents

| Type | Name | What it does |
|---|---|---|
| Skill | `oss-contribution` | Open-source contribution workflow: pick an issue, avoid duplicate work, prove the fix (fails before, passes after), publish, triage |
| Skill | `interview-pair` | Pair-programming interview habits (the 3 facts before the first edit, codebase orientation) and mock interview mode |
| Skill | `architecture` | Architecture docs with C4 Mermaid diagrams, step-by-step system design, ADRs, system-design interview practice |
| Skill | `skill-scout` | Finds existing skills (local, skills.sh, GitHub), vets them for risks, installs only after confirmation |
| Agent | `pr-test-analyzer` | Reviews whether a PR's tests actually cover the changed behavior |
| Agent | `silent-failure-hunter` | Finds swallowed errors, dangerous fallbacks, lost error propagation |

## Credits

`agents/pr-test-analyzer.md` and `agents/silent-failure-hunter.md` are adapted from
[affaan-m/ECC](https://github.com/affaan-m/ECC) (commit `ef648e0`), MIT License,
Copyright (c) 2026 Affaan Mustafa. The "3 facts" in `interview-pair` are adapted from ECC's GateGuard skill.
`skill-scout` combines ideas from [vercel-labs/skills](https://github.com/vercel-labs/skills) `find-skills` (MIT) and ECC's `skill-scout`.
`skills/architecture/references/` (C4 syntax, patterns, common mistakes) is copied from
[softaworks/agent-toolkit](https://github.com/softaworks/agent-toolkit/tree/main/skills/c4-architecture) (commit `06825f0`), MIT License.
The ADR format in `architecture` is adapted from ECC's `architecture-decision-records` skill.
