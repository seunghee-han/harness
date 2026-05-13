---
description: Prepare Etcher Solution changes for review
---

# Review Preparation Workflow

Use this workflow only when the user asks to prepare a commit, PR, or review-ready change. Remote pushes and PR creation require explicit user approval.

## Local Review

1. Inspect the worktree.
   Run `git status --short`

2. Review the diff for the files you changed.
   Run `git diff -- <changed paths>`

3. Run the smallest useful verification for the change type:
   - Documentation-only: verify paths, Markdown readability, and stale placeholders.
   - Frontend: `npm --prefix frontend run test`, plus build when needed.
   - Backend: focused tests or `pytest backend/tests` for shared changes.

4. Summarize changed files, verification results, and any residual risk.

## Commit And PR Actions

- Ask before staging broad sets of files.
- Ask before committing.
- Ask before pushing to a remote branch.
- Ask before creating a PR with `gh pr create`.
- Never use destructive Git operations unless the user explicitly asks for them.
