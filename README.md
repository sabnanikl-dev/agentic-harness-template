# Agentic Harness Template

A minimal, opinionated template for multi-agent software development.

## What This Is

This repo is a starting point for projects built with AI agents. It encodes:
- **Process rules** (`AGENTS.md`) — how agents collaborate
- **Project context** (`docs/spec.md`) — what we're building
- **Knowledge structure** (`docs/`) — where context lives

## How to Use

1. **Fork/clone this repo** for your new project
2. **Customize `docs/spec.md`** — fill in tech stack, design system, architecture
3. **Add issues** to GitHub with acceptance criteria
4. **Tell your orchestrator:** "work on issue #N"
5. **Say go/no-go** when the orchestrator reports back

## Agent Flow

```
Human: "work on issue #42"
  → Orchestrator reads issue, checks ACs
    → Builder (fresh session) implements
      → Evaluator (fresh session) reviews technically
        → [fix loop if needed, max 2 cycles]
          → Orchestrator reports to human
            → Human: "go" → merged
```

## Files

| File | Purpose |
|------|---------|
| `AGENTS.md` | Process bible. All agents read before starting. |
| `docs/spec.md` | Living project context. Updated as work progresses. |
| `docs/design/` | Brand guidelines, inspiration, screenshots |
| `docs/api/` | API schemas, endpoints |
| `docs/friction/` | Running logs — what broke and how we fixed it |
| `docs/conventions/` | Code style, naming rules |

## Principles

- **Generation ≠ evaluation** — builders don't review their own work
- **One task, one PR** — no unrelated changes bundled
- **Fresh sessions** — context comes from files, not memory
- **Repo is source of truth** — no Slack, no Notion, no Google Docs
- **Human gate is load-bearing** — no automated merges

## Sources

Based on harness engineering research from:
- [Anthropic: Harness Design for Long-Running Apps](https://www.anthropic.com/engineering/harness-design-long-running-apps)
- [Anthropic: Effective Harnesses for Long-Running Agents](https://www.anthropic.com/engineering/effective-harnesses-for-long-running-agents)
- [OpenAI: Harness Engineering](https://openai.com/index/harness-engineering/)
- [Ralph Wiggum as a Software Engineer](https://ghuntley.com/ralph/)
