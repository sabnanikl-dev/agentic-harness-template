# Agent Development Harness

*Slim process bible. All agents must read this before touching the repo.*

## Roles

| Agent | Role |
|-------|------|
| **Human** | Vision + taste. Approves via go/no-go. |
| **Orchestrator** (e.g., Hermes) | Reads issues, spawns builders, reviews, reports. |
| **Builder** (e.g., Claude Code) | Implements features. |
| **Evaluator** (e.g., Codex) | Technical review only. |

## Core Rules

1. **Generation ≠ evaluation.** Builders do not review their own work.
2. **One task, one PR, one branch.** No unrelated changes bundled.
3. **Cross-review required.** Every PR needs technical review before human approval.
4. **Human gate is load-bearing.** No automated merges.
5. **If it's not in the repo, it doesn't exist.** No Slack, no Notion, no Google Docs.

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
├── design/    # Brand, inspiration, screenshots
├── api/       # Schemas, endpoints
├── friction/  # Running logs — what broke, how fixed
└── conventions/ # Code style, naming, golden principles
```

## Golden Principles (Anti-AI-Slop)

- No hardcoded values — use design tokens / config
- No debug code in production (`console.log`, etc.)
- No unused imports or dead code
- No placeholder implementations — full implementations only
- Parse, don't validate — type safety at boundaries
- Follow existing naming conventions

## Loop Termination

- Max 2 review-fix cycles per PR. Escalate to human if exceeded.
- Evaluator outputs only BLOCKING issues with file:line references.
- Orchestrator can short-circuit false positives from evaluator.

## Verification (Non-Negotiable)

**Post-push:** `gh pr view <N> --json commits` — confirm commit landed.
**Post-merge:** `gh pr view <N> --json merged,state` — confirm `"merged": true` before reporting success.

## Communication

| Channel | Use |
|---------|-----|
| External chat | Human ↔ Orchestrator only |
| GitHub Issues | Task tracking, single source of truth |
| GitHub PRs | Code review between agents |
| Repo files | All other knowledge |

---

*This is a template. Customize docs/spec.md and docs/ for each project. Do not bloat this file.*
