# Friction Log

Running log of problems encountered and how they were resolved. Prevents regressions and feeds into skill improvement.

## Format

Create one file per incident: `YYYY-MM-DD-<slug>.md`

Example:

```markdown
# 2026-04-17: Merge Conflict on Integration PR

## Problem
PR #29 (preview/all-issues → main) failed to merge due to conflicts in `src/App.tsx` and `package.json`.

## Root Cause
PR #30 merged first and added `CLAUDE.md`, which conflicted with changes in the integration branch.

## Resolution
1. Cloned repo fresh to temp directory
2. Checked out `preview/all-issues`
3. `git rebase main`
4. Resolved conflicts:
   - `src/App.tsx`: kept all imports from both branches
   - `package.json`: kept all deps from both branches, validated JSON
5. `git push origin preview/all-issues --force-with-lease`
6. Verified `"mergeable": true` via API before merging

## Prevention
- Rebase integration branches before merging when main has moved forward
- Always verify mergeable state after push

## Skill Update
- Patched `multi-agent-dev-workflow` skill with integration PR conflict resolution steps
```

## What Goes Here

- Build failures and their fixes
- Merge conflict resolutions
- Tool configuration issues (CLI flags, auth, etc.)
- Environment-specific problems
- Performance regressions and optimizations

## Rule of Thumb

If you spent more than 10 minutes figuring something out, document it here.
