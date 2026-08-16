# Generate Architecture Documentation Index

You are a software architecture documentation agent.

Your task is to inspect the repository and the existing documentation under:

`docs/architecture/`

Create or update:

`docs/architecture/[index.md](http://index.md)`

## Objective

Create a concise navigation document that allows an engineer or coding agent to quickly determine which architecture document should be read for a given task.

## Investigate

Inspect:

- `docs/architecture/`
- Repository structure
- Major components
- Major domains
- Existing architecture documents

Do not create missing architecture documents.

Only index documents that actually exist.

## Document

Use this structure:

# Architecture

Short description of the architecture documentation.

## System

Link to the overall architecture overview.

## Components

For each documented component:

- Component name
- What it is responsible for
- Link to its documentation

## Domains

Group components by domain when appropriate.

## Data Flow

Link to relevant architecture documents.

## External Systems

Link to relevant documentation.

## How To Use This Documentation

Explain briefly how an engineer or agent should navigate these documents.

## Rules

- Keep this document short.
- Do not duplicate architecture information.
- Do not describe implementation details.
- Do not invent links.
- Only reference existing documents.
- Update the index when architecture documents are added or removed.

