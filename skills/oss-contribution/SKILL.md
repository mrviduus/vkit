---
name: oss-contribution
description: End-to-end workflow for contributing to an open-source project, from picking an issue to a PR maintainers can merge. Use when the user wants to contribute to an OSS repo, find an issue to work on, fix an issue in someone else's project, open a PR upstream, or comment on/triage issues.
---

# Open-Source Contribution

Goal: a small PR that maintainers can verify and merge quickly. Every public claim checked, no duplicate work, no guessing.

## 1. Learn the project's rules first

Before picking anything, read:
- `CONTRIBUTING.md`, the README "contributing" section, `.github/` (issue/PR templates, workflows).
- Commit convention from `git log --format=%s -30` (e.g. Conventional Commits, capitalization, scopes).
- Test layout and how to run one test locally.
- Who merges and what: `gh pr list -R <repo> --state merged --limit 30 --json author,title`. Many distinct outside authors = PR-friendly. Only maintainers = expect slow review; keep PRs tiny.

## 2. Pick an issue

```bash
gh issue list -R <repo> --label "good first issue"
gh issue list -R <repo> --state open --limit 60 --json number,title,labels,comments,updatedAt
gh pr list -R <repo> --state open --limit 100 --json number,title,author
```

A good candidate:
- Clear expected behavior, reproducible, small surface (one module).
- Testable locally (pure unit tests beat device/UI-only bugs).
- **No open PR already**: `gh search prs --repo <repo> "<issue number>"` and search by key identifiers (function/field names). Skip issues where someone said "I'm working on this" recently.
- **Not already fixed on master**: the reporter may be on an old release. `git log -S'<code from the issue>'` and read the current code. If fixed, the contribution is a comment pointing to the commit, plus optionally a regression test.

Present 2-3 candidates with trade-offs and a recommendation. Let the user choose.

## 3. Fix

- Branch from fresh `origin/master` (or `main`), one branch per PR, named `fix/...`, `test/...`, `feat/...`.
- Match surrounding code exactly: indentation, naming, comment density, existing helpers.
- Smallest change that fixes the root cause. No drive-by refactors or formatting changes.

## 4. Prove it

- **Fails before, passes after.** Revert only the fix (or apply only the test) and confirm the test fails with the issue's symptom, then passes with the fix. A test that never failed proves nothing.
- Run the whole module's test suite, not just the new test.
- For UI or Android code that can't be unit-tested: build it and run it (emulator, `adb shell am start ...`, a dummy script that logs its arguments) and record the inputs and outputs as a table.

## 5. Publish (confirm with the user before any public action)

```bash
gh repo fork <owner>/<repo> --remote --remote-name fork --clone=false
git push -u fork <branch>
gh pr create -R <owner>/<repo> --base master --head <user>:<branch> --title "..." --body-file -
```

- Commit and PR title follow the repo's convention. Reference the issue (`Closes #N` for fixes, `Refs #N` otherwise).
- PR body: what changed, why, and **how it was verified** (commands, results, before/after). Short.
- Don't ping maintainers for 2-3 weeks. First-time contributors' CI shows `action_required` until a maintainer approves the workflow run; that is not a failure or an approval.

## 6. Public comments and triage

Comments on issues are public and permanent. Before posting:
- Check every identifier, commit hash, version, and file name against the actual code or `git show`. Don't paraphrase from memory (e.g. terminfo vs termcap names).
- Say which release will include a fix: `git tag --contains <commit>`.
- Keep it to facts plus a link. If you got something wrong, edit the comment right away.

Triage comments ("fixed on master in <commit>, not released yet") are valuable contributions on their own.

## 7. Before opening the PR, review it yourself

Run the `pr-test-analyzer` agent on the diff. For code with error paths, also run `silent-failure-hunter`. Fix critical gaps or explain them in the PR body.
