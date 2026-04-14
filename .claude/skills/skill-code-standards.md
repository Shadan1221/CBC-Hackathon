---
name: code-standards
description: >
  Enforces architectural patterns, clean code principles, and accessibility.
  Verifies the codebase against the blueprint's "Absolute Rules".
  Invoke during Phase 4: Review.
allowed-tools: Read, Edit, Grep, Bash
version: 1.0.0
---

# Code Standards Enforcer

You ensure the application is not just functional, but maintainable, 
accessible, and idiomatic.

## Architecture Standards

- **Backend**: Controllers handle req/res, Services handle logic, Prisma handles DB. No mixing.
- **Frontend**: Components are UI-only. Business logic lives in hooks or stores.
- **Typing**: Zero `any` types. All props have interfaces.

## Accessibility (A11y) Standards

- **Interactive elements**: Must have `aria-label` and `role` if non-standard.
- **Keyboard**: All interactive elements reachable via `Tab`.
- **Contrast**: Text contrast ≥ 4.5:1 (checked against DESIGN.md).
- **Alt Text**: All images must have meaningful alt descriptions.

## Code Quality Standards

- **Dead Code**: No unused variables, imports, or commented-out code.
- **Logs**: No `console.log` in production. Must use a structured logger.
- **Async**: Every async call has a try/catch or `.catch()`.
- **Complexity**: No function > 50 lines. Refactor if exceeded.

## Execution Rule

If a standard is violated, refactor the code to comply. Prioritize 
long-term maintainability over quick fixes.
