---
name: review
description: Run project tests and checks, read relevant documentation and rules, then delegate the code review to a reviewer subagent using all gathered context and evidence.
---

Review the current implementation using the project's documentation, rules, tests, and actual execution results.

The goal is to **gather objective evidence first** and only then ask the `reviewer` subagent to evaluate the changes.

## Workflow

### 1. Understand the project rules

Before reviewing code, inspect the repository for project-level instructions.

Look for:

* `AGENTS.md`
* `Makefile`
* `docs/*`
* project-specific rules or guidelines

Read the relevant files before making any review decision.

Do not assume conventions that are not supported by the repository.

### 2. Understand what changed

Inspect the current Git state:

```bash
git status --short
git diff
git diff --cached
```

If the repository has a base branch or PR context available, inspect the relevant diff against that base.

Determine:

* What was changed
* Which files are affected
* What feature or bug the changes address
* Whether tests were added or modified
* Whether the implementation follows existing architectural patterns

Do not review unrelated changes.

### 3. Identify the validation commands

Inspect the project configuration to determine the correct commands.

Examples:

```bash
cat package.json
cat Makefile
cat Cargo.toml
cat pyproject.toml
```

Depending on the project, identify relevant:

* Unit tests
* Integration tests
* E2E tests
* Type checking
* Linting
* Formatting checks
* Build checks
* Static analysis

Prefer the project's existing scripts and documented commands over inventing new ones.

### 4. Run validation

Run the relevant checks.

For example:

```bash
npm test
npm run lint
npm run typecheck
npm run build
```

Or the equivalent commands for the project's stack.

Run tests that are relevant to the changed code first.

If practical, run the broader test suite afterward.

Record:

* Command executed
* Exit code
* Pass/fail result
* Important errors or warnings
* Tests that were skipped
* Tests that could not be executed and why

**Never claim that tests passed unless they actually passed.**

If a command fails because of an environmental issue rather than the implementation, clearly distinguish the two.

### 5. Read relevant documentation

Based on the changed code, read the documentation that defines the expected behavior.

Pay particular attention to:

* Feature requirements
* Architecture decisions
* API contracts
* Domain rules
* Security requirements
* Testing conventions
* Error-handling conventions
* Performance requirements

The reviewer should evaluate the implementation against the repository's actual requirements, not generic preferences.

### 6. Build the review context

Before invoking the reviewer subagent, collect the relevant information into a concise context.

Include:

```text
## Task / Feature
<what the change is supposed to accomplish>

## Project Rules
<relevant rules and constraints>

## Documentation
<relevant requirements and architectural decisions>

## Changed Files
<list of relevant files>

## Validation
<commands executed and their results>

## Test Failures
<failures, if any>

## Diff
<relevant implementation diff>

## Review Focus
<areas that deserve particular attention>
```

Do not overload the subagent with unrelated repository information.

### 7. Delegate to the reviewer subagent

Once the context has been gathered, invoke the `reviewer` subagent.

The reviewer must independently analyze the implementation using:

1. The requirements
2. The project rules
3. The documentation
4. The changed code
5. The test results
6. The actual repository state

Use the gathered evidence as context rather than asking the reviewer to blindly trust the implementation.

The reviewer should specifically look for:

* Functional bugs
* Incorrect behavior
* Missing edge cases
* Violations of project rules
* Architectural inconsistencies
* Incorrect error handling
* Security problems
* Race conditions
* Data integrity issues
* Missing or insufficient tests
* Regressions
* Unnecessary complexity
* Maintainability problems

Do **not** ask the reviewer to rewrite the implementation unless explicitly requested.

### 8. Reviewer Output

Return actionable findings grouped by severity:

```text
### Critical
- [file:line] Problem
  - Evidence
  - Suggested fix

### High
...

### Medium
...

### Low
...

### Tests
- PASS/FAIL: <command>

### Summary
<assessment>
```

### Issue Files

After the review, create files from the findings:

* **Critical:** one file per issue
* **High:** one file per issue
* **Medium:** one file per issue
* **Low:** all issues in one file
* No findings → no issue files

Use:

```text
docs/issues/
├── critical/<issue-slug>.md
├── high/<issue-slug>.md
├── medium/<issue-slug>.md
└── low.md
```

Each issue file must contain:

```markdown
# [SEVERITY] <title>

## Location
`<file>:<line>`

## Problem
<description>

## Evidence
<evidence>

## Suggested Fix
<concrete fix>
```

## Important Rules

* **Evidence before judgment.**
* Always read applicable project rules before reviewing.
* Always inspect the actual diff.
* Run relevant tests/checks before invoking the reviewer.
* Never fabricate test results.
* Never assume a test passes because it exists.
* Distinguish implementation failures from environment failures.
* Keep the reviewer focused on the current change.
* Prefer repository-specific conventions over generic best practices.
* The reviewer is responsible for the final code-quality assessment.
* The parent agent is responsible for gathering accurate context and validation evidence.

## Reviewer Prompt

When invoking the subagent, use a prompt equivalent to:

> You are reviewing an implementation.
>
> Review the provided change against the project's requirements, documentation, rules, and actual validation results.
>
> Do not assume the implementation is correct because tests pass. Analyze the diff independently.
>
> Prioritize real defects and actionable issues over stylistic preferences.
>
> For every finding, provide the file and line, explain why it is a problem, provide evidence from the code or requirements, and suggest a concrete fix.
>
> Pay particular attention to regressions, edge cases, incorrect assumptions, architecture violations, security issues, error handling, and missing tests.
>
> If you find no meaningful issues, explicitly state that no blocking issues were found.
>
> Here is the complete review context:
>
> [INSERT GATHERED CONTEXT]

## Final Decision

After the reviewer returns:

* If **Critical** or **High** findings exist, report them clearly.
* If only **Medium/Low** findings exist, report them but do not treat them as blockers unless project rules require it.
* If no issues exist and validation passes, report that the implementation is ready.
* Never modify code automatically based solely on reviewer feedback unless the user explicitly requested an automatic fix cycle.