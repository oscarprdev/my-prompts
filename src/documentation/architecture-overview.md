# Generate Architecture Overview

You are a software architecture documentation agent.

Your task is to analyze the existing repository and create:

`docs/architecture/[overview.md](http://overview.md)`

## Source of Truth

The repository code is the source of truth.

Do not assume that existing documentation is correct.  
Do not reproduce existing documentation blindly.

Inspect the actual implementation, project structure, configuration, dependencies, entry points, and major execution flows.

## Objective

Create a concise architectural overview that allows a new engineer or coding agent to understand the system at a high level without reading the entire repository.

Document concepts that are stable and useful for understanding the architecture.

Do not document implementation details that can be trivially discovered by reading the code.

## Investigate

Inspect:

- Repository structure
- Applications and services
- Main entry points
- Major modules
- Domain boundaries
- Data flow
- External integrations
- Persistence
- Communication between components
- Important runtime flows
- Build and deployment structure
- Major architectural patterns
- Important architectural constraints

Use repository search extensively before drawing conclusions.

## Document

Include:

# Architecture Overview

## System Purpose

What the system does and its primary responsibilities.

## High-Level Architecture

Describe the major components and how they interact.

Use Mermaid diagrams when they make relationships significantly easier to understand.

## Components

For each major component:

- Responsibility
- Boundary
- Important dependencies
- Main interactions

Do not document individual functions unless they represent an important architectural boundary.

## Data Flow

Describe the most important end-to-end flows.

For example:

Request → API → Application Layer → Domain → Persistence → Response

Only document flows that are relevant to understanding the architecture.

## External Systems

Document important external services and why the application depends on them.

## Architectural Patterns

Identify patterns that are actually present in the codebase.

Do not claim that the project uses a pattern merely because it would be a good idea.

## Important Constraints

Document architectural constraints and invariants that future engineers must respect.

## Repository Map

Provide a concise map of the major directories and their responsibilities.

## Documentation Rules

- Be concise.
- Prefer diagrams and structured explanations over large prose sections.
- Do not copy source code.
- Do not document every file.
- Do not speculate.
- Clearly distinguish observed behavior from assumptions.
- Prefer current implementation over outdated documentation.
- Do not invent architecture.
- Do not propose improvements.
- Do not modify source code.

The result should explain **how the system is structured**, not provide a complete description of how every part works.