# Git Workflow Notes

## Why the repository is the source of truth (not the org)
- Every change in the repo has a history (commits) — who changed what, and why.
- Org changes made directly can be lost or forgotten with no record.
- Mistakes can be undone easily in Git (revert/rollback); undoing org changes is harder.
- The whole team works from the same codebase via pull/push, avoiding sync issues.
- Changes can be reviewed (pull requests) before merging — direct org edits skip this check.

## Ignored local files
- `.sf/`, `.sfdx/`, `.env`, and any credential files must always be excluded via `.gitignore` — no exceptions, ever.