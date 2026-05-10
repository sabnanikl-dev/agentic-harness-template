# Agent Development Harness

*Slim process bible. All agents must read this before touching the repo.*

## Roles

| Agent | Role |
|-------|------|
| **Human** | Vision + taste. Approves via go/no-go. |
| **Orchestrator** (e.g., Hermes) | Reads issues, spawns builders, reviews, reports. |
| **Builder** (e.g., Claude Code) | Implements features and keeps repo docs accurate when implementation changes the contract. |
| **Evaluator** (e.g., Codex) | Technical review only; flags stale or missing docs when they would mislead the next builder. |

## Core Rules

1. **Generation ≠ evaluation.** Builders do not review their own work.
2. **One task, one PR, one branch.** No unrelated changes bundled.
3. **Cross-review required.** Every PR needs technical review before human approval.
4. **Human gate is load-bearing.** No automated merges.
5. **If it's not in the repo, it doesn't exist.** No Slack, no Notion, no Google Docs.

## Template vs Task Harness

Before editing, identify whether this repo is still the reusable template or has been cloned for a specific task/client.

If this is the reusable template:
- Keep examples generic.
- Do not add client names, Linear issue IDs, pilot-specific status files, or one-off reviewer verdicts.
- Use placeholders instead of task-specific facts.

If this has been cloned for a specific task/client:
- Task-specific notes are allowed in the appropriate `docs/`, `outputs/`, or evidence folders.
- Keep approval-needed items clearly labeled.
- Do not perform live/client/account changes unless explicitly approved.

## Session Start (Orientation)

Every session begins with explicit state reconstruction:

```
1. git status — confirm working directory
2. Read AGENTS.md — refresh process rules
3. Read docs/spec.md — refresh project context
4. Read assigned issue — refresh acceptance criteria
5. git log --oneline -20 — see recent work
6. Run build/test baseline — verify clean state
```

**Fresh session every time.** No `--continue`. Context comes from files, not previous session memory.

## Branch Naming

| Prefix | Use |
|--------|-----|
| `feat/issue-N` | New feature |
| `fix/issue-N` | Bug fix |
| `chore/` | Tooling, config, deps |
| `hotfix/` | Urgent production fix |

## Commit & PR

- Commit per logical change. Descriptive messages.
- Open PR with linked issue, summary, and acceptance criteria check.
- Self-review your diff before requesting cross-review.
- Builder never merges their own PR.

## Knowledge Structure

```
AGENTS.md      # This file — process rules (slim, reusable)
docs/
├── spec.md    # Project context (living document)
├── build-plan.md # Lean phase / issue-slicing plan
├── design/    # Brand, inspiration, screenshots
├── api/       # Schemas, endpoints
├── friction/  # Running logs — what broke, how fixed
└── conventions/ # Code style, naming, golden principles
```

## Harness Stewardship

Once work starts, the harness is a working contract. Builder and evaluator jointly own keeping repo docs accurate enough for the next fresh session.

Update docs in the same PR when a change alters scope, setup, file paths, conventions, verification, approval gates, or known friction. If a doc cannot be fully reconciled, leave a clear `TODO:` with the missing decision/source instead of letting drift hide.

Files that commonly drift:
- `AGENTS.md` — process rules proven by practice
- `docs/spec.md` — scope, constraints, approval gates, AC
- `docs/build-plan.md` — phase plan and sequencing, when present
- `docs/conventions/` — code style, naming, patterns
- `docs/friction/` — non-obvious breakage and prevention

Skills, tools, and agent-specific references are optional accelerators unless explicitly listed as project requirements. Required constraints must live in repo docs and issues, not in one agent's private memory or local skill library.

If the harness is wrong, fix the harness — do not work around it silently.

## Golden Principles (Anti-AI-Slop)

- No hardcoded values — use design tokens / config
- No debug code in production (`console.log`, etc.)
- No unused imports or dead code
- No placeholder implementations — full implementations only
- Parse, don't validate — type safety at boundaries
- Follow existing naming conventions
- **Search before implementing** — use `rg` / `grep` to check if similar code already exists

## Loop Termination

- **One cycle = review + fix.** Max 2 cycles total (initial build + 2 review rounds).
- Escalate to human if exceeded.
- Evaluator outputs only BLOCKING issues with file:line references.
- Orchestrator can short-circuit false positives from evaluator.

## Merge Conflicts

- **Trivial conflicts** (lockfiles, generated code, import order) — auto-resolve, keep all changes, regenerate if needed.
- **Semantic conflicts** (business logic, component APIs, schema changes) — escalate to human with summary of each branch's changes and a recommended action.

## Verification (Non-Negotiable)

**Post-push:** `gh pr view <N> --json commits` — confirm commit landed.
**Post-merge:** re-query the PR via REST and confirm `"merged": true` before reporting success:

```bash
gh api repos/<owner>/<repo>/pulls/<N> --jq '{state, merged, merged_at, merge_commit_sha}'
```

## Communication

| Channel | Use |
|---------|-----|
| External chat | Human ↔ Orchestrator only |
| GitHub Issues | Task tracking, single source of truth |
| GitHub PRs | Code review between agents |
| Repo files | All other knowledge |

---

*This is a template. Customize docs/spec.md and docs/ for each project. Do not bloat this file.*
