---
name: implement
description: Implement a plan after explicitly selecting and reading the documentation required by that plan.
---

# Implement

Implement the provided plan. **Do not modify code until the required documentation has been identified and read.**

## Workflow

### 1. Read the Plan

Understand the plan completely:

- Goals
- Scope
- Files/components affected
- Architecture decisions
- Tests and validation requirements

Do not reinterpret or expand the plan without justification.

### 2. Select Required Documentation

Before touching code, determine the **main documentation files required to implement the plan**.

Output a documentation checklist:

| File | Why it is required |
|---|---|
| `docs/...` | What part of the plan it governs |

You must include:

- Relevant product/architecture specifications
- Relevant ADRs
- **Every applicable convention file**
- Testing conventions when tests are involved

If the scope is unclear, read the relevant overview documentation first.

### 3. Read the Documentation

Read every selected documentation file **in full** before implementation.

Do not rely on filenames, summaries, previous knowledge, or assumptions.

If required documentation is missing, contradictory, or unclear:

**STOP and report the documentation failure. Do not implement.**

### 4. Implement

After documentation is read:

1. Inspect the existing implementation.
2. Follow the plan and documented conventions.
3. Make the smallest changes required.
4. Reuse existing patterns before introducing new ones.
5. Do not introduce architecture or behavior outside the plan unless required.
6. Keep the implementation consistent with the documentation.

## Hard Rules

- **Documentation before code.**
- **You must explicitly choose the required documentation files.**
- **You must read the chosen files before modifying code.**
- **Every applicable convention is mandatory.**
- **Never implement based only on the plan or existing code.**
- **Never skip documentation because the change appears small.**
- **If required documentation cannot be read, stop.**