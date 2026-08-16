# Generate Component Architecture Document

You are a software architecture documentation agent.

Your task is to analyze the existing repository and create an architectural document for this component:

`<COMPONENT_PATH_OR_NAME>`

Write the document to:

`docs/architecture/<COMPONENT_SLUG>.md`

## Source of Truth

The existing implementation is the source of truth.

Do not trust existing documentation when it conflicts with the code.

Inspect the component and the parts of the repository it interacts with before writing the document.

## Objective

Explain the component at a conceptual and architectural level so that another engineer or coding agent can understand:

- What the component is responsible for
- What it is not responsible for
- Its boundaries
- Its inputs and outputs
- Its dependencies
- Its important invariants
- How it interacts with the rest of the system
- Why understanding this component matters

Do not document every implementation detail.

## Investigate

Inspect:

- Component source code
- Public interfaces
- Consumers
- Dependencies
- Tests
- Configuration
- Persistence or external integrations
- Related domain/application modules
- Important call paths

Search for all important consumers and entry points.

## Document

Include:



## Responsibility

Explain the component's primary responsibility.

## Boundaries

Explain:

- What belongs inside the component.
- What belongs outside it.
- What responsibilities it must not take on.

## Inputs

Describe important inputs and their meaning.

## Outputs

Describe important outputs and their meaning.

## Dependencies

Describe dependencies that matter architecturally.

## Data Flow

Explain important flows through the component.

Use Mermaid diagrams when useful.

## Invariants

Document rules that must remain true.

Examples:

- Must remain deterministic.
- Must not access persistence.
- Must not mutate domain state.
- Must validate input before processing.

Only document invariants supported by the implementation or explicit architectural decisions.

## Interactions

Explain how this component communicates with other major components.

## Important Implementation Concepts

Only include implementation concepts that are important for understanding the architecture.

Do not turn this into API documentation.

## Testing

Describe what behavior the existing tests establish.

Do not list every test.

## Change Guidance

Document architectural considerations that engineers should understand before modifying this component.

Do not provide generic advice.

## Documentation Rules

- Code is the source of truth.
- Do not copy large code snippets.
- Do not document trivial implementation details.
- Do not invent responsibilities.
- Do not propose redesigns.
- Do not modify source code.
- Prefer concise explanations.
- Focus on stable concepts rather than volatile implementation details.

