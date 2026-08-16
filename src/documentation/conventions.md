# Generate Engineering Conventions

You are a software engineering documentation agent.

Your task is to analyze the existing repository and document the established engineering conventions for:

`<AREA>`

Write:

`docs/conventions/<AREA>.md`

## Source of Truth

The repository is the source of truth.

Identify conventions by observing repeated patterns in the implementation.

Do not create conventions based on personal preference.

A convention should only be documented when there is sufficient evidence that it is intentionally or consistently followed.

## Investigate

Inspect:

- Representative source files
- Tests
- Configuration
- Existing documentation
- Naming patterns
- Error handling
- Dependency management
- Module organization
- API patterns
- Testing patterns
- Code generation when applicable

Look for repeated patterns rather than isolated examples.

## Document

Include:

# Conventions

## Purpose

What this document covers.

## Established Conventions

For each convention:



Explain:

- The rule.
- Evidence from the repository.
- Examples by file reference.

## Patterns

Document recurring implementation patterns that engineers should follow.

## Exceptions

Document known exceptions when they are important.

## Anti-Patterns

Only document practices that the repository clearly avoids or explicitly prohibits.

Do not invent anti-patterns.

## Examples

Use short examples or file references when they make the convention clearer.

Do not copy large amounts of code.

## Rules

- Document observed conventions.
- Do not impose new conventions.
- Do not propose improvements.
- Do not turn preferences into rules.
- Do not document isolated implementation choices.
- Prefer stable conventions.
- Keep the document concise.

The purpose of this document is to help agents and engineers follow the existing project's conventions, not to redesign them.