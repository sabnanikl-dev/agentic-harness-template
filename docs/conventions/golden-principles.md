# Golden Principles (Anti-AI-Slop)

Mechanical rules. Not suggestions. Enforced at every layer.

## Code Quality

- **No hardcoded values** — use design tokens, config files, or environment variables
- **No debug code in production** — `console.log`, `debugger`, `alert`, etc. must be removed before commit
- **No unused imports or dead code** — every import must be used; every function must be called
- **No placeholder implementations** — full implementations only; no `TODO` or `FIXME` in committed code
- **Parse, don't validate** — type safety at boundaries; use schemas (Zod, io-ts) instead of manual checks
- **Search before implementing** — use `rg` / `grep` to check if similar code already exists before writing new code

## Naming & Structure

- Follow existing naming conventions in the codebase
- One component per file (React/Vue/Svelte)
- Co-locate tests with source files or in `__tests__/` directories consistently
- Keep functions small and focused; extract when logic exceeds ~30 lines

## Styling (Tailwind / CSS)

- No arbitrary Tailwind values `[]` — extend the config instead
- Use design system tokens for colors, spacing, typography
- No inline styles except for dynamic values computed at runtime

## Dependencies

- Don't add new deps without checking if the project already has a solution
- Pin versions in `package.json` for stability
- Update `package-lock.json` / `yarn.lock` / `pnpm-lock.yaml` in the same PR

## Enforcement Layers

1. **Pre-write** — AGENTS.md + docs/ progressive disclosure teach conventions
2. **Pre-commit** — ESLint/Prettier/Stylelint catch violations locally
3. **Pre-review** — executing agent self-reviews its own diff
4. **Cross-review** — other agent reviews the PR diff
5. **Visual QA** — orchestrator screenshots key pages, catches visual regressions

## When in Doubt

- Look at the 3 most recent commits on `main` — follow their patterns
- Check `docs/friction/` for previous decisions on similar problems
- Ask the human if the convention is unclear
