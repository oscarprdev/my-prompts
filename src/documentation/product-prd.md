# Generate [PRD.md](http://PRD.md)

You are a product documentation agent.

Analyze the existing project and create:

`docs/product/[PRD.md](http://PRD.md)`

## Source of Truth

Use the current repository, existing product documentation, UI, APIs, domain models, and implemented behavior as evidence.

Do not invent product capabilities that are not supported by the project.

The PRD should describe the product at the product level, not its implementation details.

## Objective

Create a clear PRD that explains what the product is trying to achieve and what capabilities it provides.

## Investigate

Inspect:

- Existing application behavior
- Main user flows
- UI and UX
- Public APIs when relevant
- Domain concepts
- Existing documentation
- Existing features
- Important product constraints

Distinguish implemented behavior from assumptions.

## Output

Create `docs/product/[PRD.md](http://PRD.md)` with:

# Product Requirements Document

## Product Overview

## Problem

## Goals

## Target Users

## Core User Journeys

## Functional Requirements

Organize requirements by product capability.

## Non-Functional Requirements

Only include requirements that are product-relevant.

## Constraints

## Out of Scope

## Success Criteria

## Current Capabilities

Clearly distinguish existing functionality from desired future behavior.

## Open Questions

Only include questions that cannot be answered from the repository.

## Rules

- Focus on product behavior and value.
- Do not describe internal implementation details.
- Do not invent features.
- Do not turn technical architecture into product requirements.
- Keep requirements clear and testable where possible.
- Keep the document concise.
- Do not modify source code.

