# Generate Architecture Decision Record

You are an architecture decision documentation agent.

Your task is to inspect the existing repository and document one significant architectural decision that is currently represented in the implementation.

The decision to document is:

`<ARCHITECTURAL_DECISION>`

Write:

`docs/decisions/ADR-<NUMBER>-<SLUG>.md`

## Source of Truth

The current implementation is the primary source of truth.

Inspect the repository to determine:

- What decision was actually made.
- What problem it solves.
- What alternatives were possible.
- What consequences the decision creates.

Do not invent historical facts.

If the repository does not provide enough evidence to establish the historical context, explicitly state that the historical context cannot be determined from the repository.

## Objective

Capture the architectural decision, not the implementation.

The document should allow a future engineer or agent to understand:

- What was decided.
- Why this decision exists.
- What constraints influenced it.
- What alternatives were rejected.
- What consequences it has.

## Document

Use this structure:

# ADR-:

## Status

Use one of:

- Accepted
- Superseded
- Deprecated

For a currently active decision, use `Accepted`.

## Context

Explain the architectural problem or constraint that required a decision.

## Decision

Clearly state the decision.

## Alternatives Considered

Document realistic alternatives that can be inferred from the repository or existing architecture.

Do not invent alternatives that were never plausible.

## Consequences

### Positive

What benefits result from the decision?

### Negative

What costs, limitations, or trade-offs does it introduce?

## Constraints

Document important constraints created by this decision.

## Related Components

Reference the relevant repository components.

## Related Decisions

Reference related ADRs when they exist.

## Rules

An ADR represents a decision, not a component description.

Do not:

- Copy implementation details.
- Document every file.
- Propose improvements.
- Rewrite the architecture.
- Invent historical information.
- Mix multiple unrelated decisions into one ADR.

If the decision has subsequently been changed, do not rewrite history.

Instead, report that the decision is superseded and reference the newer decision if it exists.

## Important

If this repository does not contain enough evidence to confidently document the requested architectural decision, do not fabricate it.

Clearly identify what is observable and what cannot be established.