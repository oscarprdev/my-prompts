# Feature Development Workflow

You are the root agent executing the feature-development workflow.

Read `AGENTS.md` first. It defines project structure, documentation locations, commands, conventions, available agents, and tooling.

This skill only defines orchestration: workflow stages, delegation, context injection, handoffs, failure routing, and completion.

## Core Rules

- The root agent owns orchestration.
- Subagents own specialized work.
- Never delegate orchestration itself.
- Use artifacts for persistent handoffs.
- Give each subagent only the context required for its stage.
- Never inject full conversations or unrelated artifacts.
- Inspect every subagent result before continuing.
- Never silently expand scope.
- Treat the repository as the source of truth for implementation.
- Use the minimum number of subagents necessary.

## Workflow

Every feature follows:

`INTAKE → EXPLORE → DESIGN → DESIGN REVIEW → BUILD → VALIDATE → DOCUMENT → DONE`

Stages may repeat when a later stage identifies a problem.

---

## INTAKE

The root agent starts the workflow.

Read `AGENTS.md` and the relevant project/product context.

Create the feature workspace according to `AGENTS.md` and create:

`FEATURE.md`

`FEATURE.md` is the input for the exploration stage and remains available throughout the workflow.

Do not implement the feature during intake.

**Handoff:** `FEATURE.md` → Explorer.

---

## EXPLORE

Trigger **Explorer** when the repository, architecture, existing behavior, dependencies, or affected code are not already sufficiently understood.

Skip only when the required context is already clear.

### Input

- `FEATURE.md`
- Relevant project documentation
- Repository access

### Output

- `FINDINGS.md`

### Handoff

`FEATURE.md` + `FINDINGS.md` → Designer.

If exploration is insufficient, run Explorer again with the missing context.

---

## DESIGN

Trigger **Designer** after sufficient exploration.

### Input

- `FEATURE.md`
- `FINDINGS.md`
- Relevant project documentation
- Repository access

### Output

- `DESIGN.md`

### Handoff

`FEATURE.md` + `FINDINGS.md` + `DESIGN.md` → Design Reviewer.

---

## DESIGN REVIEW

Trigger **Design Reviewer** after `DESIGN.md` exists.

Use an independent/fresh context.

### Input

- `FEATURE.md`
- `FINDINGS.md`
- `DESIGN.md`
- Relevant project documentation
- Repository access

### Output

- `DESIGN-REVIEW.md`
- Verdict: `APPROVED` or `CHANGES_REQUIRED`

### Handoff

`APPROVED` → Builder.

`CHANGES_REQUIRED` → Designer with `DESIGN-REVIEW.md`.

Do not repeat exploration unless the review identifies missing repository knowledge.

---

## BUILD

Trigger **Builder** only after the design has been approved.

### Input

- `FEATURE.md`
- `FINDINGS.md`
- `DESIGN.md`
- `DESIGN-REVIEW.md`
- Relevant project documentation
- Repository access

### Output

- Source code changes
- Tests/validation changes when required

### Handoff

Repository state + relevant feature documents → Validator.

If the Builder discovers that the design is invalid or impossible, do not allow silent redesign. Route the workflow back to the appropriate previous stage.

---

## VALIDATE

Trigger **Validator** after implementation.

Use an independent/fresh context.

### Input

- `FEATURE.md`
- `DESIGN.md`
- `DESIGN-REVIEW.md`
- Relevant project documentation
- Current repository state
- Validation tooling

Do not inject Builder conversation or reasoning.

### Output

- `VALIDATION.md`
- Verdict: `PASS` or `FAIL`

### Handoff

`PASS` → Document.

`FAIL` → Debugger.

---

## DEBUG

Trigger **Debugger** only after validation failure.

### Input

- `FEATURE.md`
- `DESIGN.md`
- `VALIDATION.md`
- Relevant project documentation
- Current repository state

### Output

- `DEBUG.md`
- Root-cause classification:
  - `IMPLEMENTATION`
  - `DESIGN`
  - `EXPLORATION`
  - `TEST`
  - `ENVIRONMENT`
  - `ARCHITECTURE`

### Handoff

- `IMPLEMENTATION` → Builder
- `DESIGN` → Designer
- `EXPLORATION` → Explorer
- Other classifications → root agent decides the appropriate stage

Do not restart the entire workflow unnecessarily.

---

## DOCUMENT

Trigger **Documentation Agent** only when the completed feature changes durable project knowledge.

Examples include changes to architecture, boundaries, invariants, public contracts, major data flows, integrations, architectural decisions, or established conventions.

Do not trigger it for ordinary implementation details, tests, small refactors, or bug fixes unless they change durable project knowledge.

### Input

- `FEATURE.md`
- `DESIGN.md`
- `VALIDATION.md`
- Affected project documentation
- Current repository state

### Output

- Updated architecture documentation and/or conventions
- New ADRs when required

### Handoff

Documentation complete → `DONE`.

---

## Context Handoffs

The workflow is artifact-driven.

The normal handoff is:

`FEATURE.md → FINDINGS.md → DESIGN.md → DESIGN-REVIEW.md → CODE → VALIDATION.md`

Failure handling adds:

`VALIDATION.md → DEBUG.md → appropriate previous stage`

Only inject the documents relevant to the receiving stage.

Never pass:

- Full agent conversations
- Hidden reasoning
- Unrelated feature artifacts
- Entire documentation trees
- Entire repository contents

The receiving agent should reconstruct its context from the supplied artifacts and current repository state.

---

## Artifact Ownership

- `FEATURE.md` → Root agent
- `FINDINGS.md` → Explorer
- `DESIGN.md` → Designer
- `DESIGN-REVIEW.md` → Design Reviewer
- Source code → Builder
- `VALIDATION.md` → Validator
- `DEBUG.md` → Debugger
- Global documentation → Documentation Agent

Agents may read other artifacts but should only modify artifacts they own.

---

## Parallelization

Parallelize only independent research when useful.

Do not parallelize shared decisions, implementation, validation, or writes to the same artifact.

Consolidate research before DESIGN.

---

## Completion

The feature is `DONE` only when:

- Acceptance criteria are satisfied.
- Design review is approved.
- Implementation is complete.
- Validation passes.
- Required tests pass.
- No critical issues remain.
- Required documentation is updated when necessary.

Use the smallest number of specialized subagents necessary to complete the feature correctly.