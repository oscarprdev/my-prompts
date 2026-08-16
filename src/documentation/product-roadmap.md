# Generate [roadmap.md](http://roadmap.md)

You are a product planning agent.

Analyze the existing project and its `docs/product/[PRD.md](http://PRD.md)` and create:

`docs/product/[roadmap.md](http://roadmap.md)`

## Source of Truth

Use:

- `docs/product/[PRD.md](http://PRD.md)`
- Existing product capabilities
- Current repository state
- Existing TODOs or incomplete capabilities when relevant

Do not invent arbitrary features.

The roadmap must reflect product priorities, not implementation tasks.

## Objective

Create a high-level product roadmap that explains what the product should focus on next.

The roadmap should help the root feature workflow determine whether a requested feature aligns with the product direction.

## Output

Create `docs/product/[roadmap.md](http://roadmap.md)` with:

# Product Roadmap

## Current State

Briefly describe the product's current maturity and capabilities.

## Now

Highest-priority product outcomes currently being pursued.

## Next

Important capabilities that should follow.

## Later

Longer-term opportunities supported by the product direction.

## Completed

Important product capabilities already delivered.

## Priorities

Explain the principles used to prioritize roadmap items.

## Out of Scope

Important areas intentionally excluded from the current direction.

## Rules

- Describe product outcomes, not implementation tasks.
- Do not create Jira/Linear-style task lists.
- Do not invent arbitrary deadlines.
- Do not assign exact dates unless the project contains explicit evidence.
- Keep initiatives broad enough to contain multiple features.
- Order initiatives by priority.
- Derive the roadmap from the PRD and current product state.
- Do not modify source code.

