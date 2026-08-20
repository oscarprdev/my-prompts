# Contributing

Thank you for your interest in this repository.

This repository contains prompt definitions and workflow documentation. It does not contain application code.

## How to contribute

1. Fork the repository.
2. Create a branch for your change.
3. Make the change.
4. Add tests if the change is logic.
5. Submit a pull request.

## What we accept

- New agent definitions. Add the prompt to `src/agents/`.
- New documentation templates. Add them to `src/documentation/`.
- New skills. Add them to `src/skills/<name>/SKILL.md`.
- Corrections to existing prompts.
- Improvements to the README or this workflow description.

## Prompt style

- Keep each prompt focused on one responsibility.
- Give the agent clear input, output, and constraints.
- Do not add duplicate content. Point to existing files instead.
- Prefer the simplest wording that states the requirement exactly.
- Prompts must be written in Simplified Technical English.

## Commit messages

Use conventional commit style:

- `feat:` for a new capability.
- `fix:` for a correction.
- `docs:` for documentation.

## Code review

A maintainer reviews the pull request. The reviewer checks clarity, usefulness, and consistency with the existing workflow.

## Code of conduct

All contributors must follow the [Code of Conduct](CODE_OF_CONDUCT.md).