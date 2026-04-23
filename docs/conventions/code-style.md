# Code Style Guide

*Project-specific style rules. Customize for your tech stack.*

## TypeScript / JavaScript

- Strict mode enabled (`"strict": true` in `tsconfig.json`)
- Prefer `const` over `let`; never use `var`
- Explicit return types on exported functions
- No `any` casts without inline comment explaining why
- Use `??` (nullish coalescing) instead of `||` for defaults

## React (if applicable)

- Functional components only
- Props interface named `{ComponentName}Props`
- Use `useCallback` / `useMemo` only when profiling shows a need
- Co-locate styles with component or use CSS modules / Tailwind

## Python (if applicable)

- PEP 8 compliant
- Type hints on all function signatures
- `ruff` for linting, `black` for formatting
- `mypy --strict` for type checking

## Testing

- Unit tests for pure logic
- Integration tests for API endpoints
- E2E tests for critical user flows only
- Every bug fix must include a regression test

## Git

- Commit messages: imperative mood, lowercase, no period
  - Good: `add contact form validation`
  - Bad: `Added contact form validation.`
- One logical change per commit
- Rebase feature branches before opening PR
