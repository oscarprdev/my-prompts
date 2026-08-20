# Agent Workflow

A documentation-first workflow that builds reliable software features with coding agents and specialized subagents.

This repository defines a practical approach to **agent orchestration, context engineering, feature specification, architecture documentation, and independent validation**.

The goal is not to maximize the number of agents.

The goal is to give each agent **the right responsibility, the right context, and the right handoff**.

The workflow is harness-agnostic. It works in Claude Code, Codex, pi, and any agents that support subagents.

---

## The Core Idea

The workflow uses a few simple principles:

- **Specialize agents.** One agent does not do everything.
- **Isolate context.** A single conversation does not grow without limit.
- **Use artifacts as handoffs.** Agents pass files, not full conversations.
- **Separate planning, implementation, and validation.**
- **Keep architectural knowledge persistent** and feature knowledge traceable.
- **Use the simplest workflow that reliably solves the task.**
- **Treat the repository as the source of truth** for current implementation.

---

## Repository Structure

```text
src/
├── agents/           # Prompt definitions for each specialized subagent
│   ├── explorer.md
│   ├── designer.md
│   ├── design-reviewer.md
│   ├── builder.md
│   ├── validator.md
│   ├── debugger.md
│   ├── reviewer.md
│   └── documenter.md
│
├── documentation/    # Prompt templates that generate project documentation
│   ├── product-prd.md
│   ├── product-roadmap.md
│   ├── architecture-overview.md
│   ├── architecture-component.md
│   ├── architecture-index.md
│   ├── audit.md
│   ├── conventions.md
│   └── decisions.md
│
└── skills/           # Orchestration skills for the root agent
    ├── implement-feature/SKILL.md
    ├── implement/SKILL.md
    ├── review/SKILL.md
    └── roast-me/SKILL.md
```

---

## Agent Roles

The root agent acts as the **orchestrator**. Specialized subagents perform the actual work.

| Role | Responsibility |
|---|---|
| Explorer | Investigate the existing codebase |
| Designer | Produce the implementation design |
| Design Reviewer | Independently challenge the design |
| Builder | Implement the approved design |
| Validator | Independently verify the implementation |
| Debugger | Diagnose validation failures |
| Reviewer | Review changes against evidence |
| Documenter | Update durable architectural knowledge |

---

## Workflow

Every feature follows a controlled lifecycle:

```text
INTAKE → EXPLORE → DESIGN → DESIGN REVIEW → BUILD → VALIDATE → DOCUMENT → DONE
```

A stage may repeat when a later stage discovers a problem.

When validation fails, the workflow does not restart from the beginning. The failure is diagnosed and routed to the stage responsible for its root cause.

## Artifact-Driven Handoffs

Agents do not pass their complete conversations to each other.

Each stage produces a persistent artifact:

```text
FEATURE.md
    ↓
FINDINGS.md
    ↓
DESIGN.md
    ↓
DESIGN-REVIEW.md
    ↓
Implementation
    ↓
VALIDATION.md
    ↓
Documentation
```

This provides:

- Context isolation
- Explicit contracts
- Persistent state
- Traceable decisions
- Smaller prompts
- Easier retries
- Better reproducibility

Large or noisy information stays outside the active model context. It is loaded only when required.

---

## Documentation Architecture

Project knowledge is separated by purpose.

```text
docs/
├── product/
│   ├── PRD.md
│   ├── roadmap.md
│   └── principles.md
│
├── architecture/
│   ├── index.md
│   ├── overview.md
│   └── ...
│
├── decisions/
│   └── ADR-*.md
│
└── conventions/
    └── *.md

specs/
└── <feature>/
    ├── FEATURE.md
    ├── FINDINGS.md
    ├── DESIGN.md
    ├── DESIGN-REVIEW.md
    ├── VALIDATION.md
    └── DEBUG.md
```

### Documentation types

- **Product** describes what the product tries to achieve.
- **Architecture** describes how the system is structured.
- **Decisions** describe why important architectural decisions were made.
- **Conventions** describe established engineering practices.
- **Specs** describe how a specific feature is researched, designed, implemented, and validated.

The code is the source of truth for current implementation. Documentation records durable knowledge that is difficult to infer from code.

### Architecture Decision Records

Important decisions are recorded as ADRs. An ADR captures:

- Context
- Decision
- Alternatives
- Consequences
- Constraints
- Status

Historical decisions are not rewritten when the architecture changes. A new ADR supersedes the previous one. This preserves the evolution of the architecture.

---

## Understanding the Workflow

### Why subagents?

- Context isolation
- Specialization
- Independent validation
- Parallel research

A subagent is not necessarily a more capable model. Its main value is a clean context window dedicated to one problem.

### Why independent validation?

The agent that designs or implements a feature does not validate its own work:

```text
Designer
   ↓
Design Reviewer

Builder
   ↓
Validator
```

Reviewers receive an independent context. They challenge the previous stage instead of continuing it.

### Model assignment

The workflow is model-agnostic. Different stages can use different models:

```text
Exploration / high-volume research  →  cheap, fast model

Design / implementation            →  strong reasoning model

Review / validation                →  independent model
```

Use **capability-to-task matching** instead of the strongest model everywhere. Measure the result with cost, latency, first-pass success, retry rate, context usage, and overall task success.

---

## Design Principles

1. **Minimize context, not information.** Keep information as artifacts. Inject it only when needed.
2. **Parallelize research, not decisions.** Investigation can run in parallel. Ownership stays explicit.
3. **One owner per artifact.** One agent owns each artifact. Others may read it but do not silently rewrite it.
4. **Prefer deterministic workflows.** Explicit stages and gates work well for well-defined processes.
5. **Fail locally.** A failure returns to the stage responsible for its root cause.
6. **Keep durable knowledge outside the conversation.** The filesystem becomes part of the agent's working memory.

---

## References

This workflow is a synthesis of established work on agent orchestration, context engineering, software architecture, and agent evaluation.

### Agent Workflows

- **Anthropic — Building Effective Agents**: Foundational work on agent architectures, including sequential workflows, parallelization, routing, orchestrator-worker patterns, and evaluator-optimizer systems.
- **Anthropic — Building Effective AI Agents: Architecture Patterns**: Recent guidance on single-agent systems, multi-agent orchestration, context management, modularity, and Skills.
- **LangChain — How and When to Build Multi-Agent Systems**: When multi-agent architectures are useful and why context engineering matters.
- **LangChain — Subagents**: Supervisor/subagent architectures, input context, output contracts, routing, and context isolation.

### Context Engineering

- **LangChain — Context Engineering for Agents**: The strategies of writing, selecting, compressing, and isolating context.
- **LangChain — How Agents Can Use Filesystems for Context Engineering**: Using filesystems to offload context and preserve intermediate work.
- **LangChain — Context Management for Deep Agents**: Compression, filesystem offloading, summarization, and long-running workflows.
- **LangChain — Using Skills with Deep Agents**: Skills as reusable instruction and context packages.

### Coding Subagents

- **Claude Code — Custom Subagents**: Specialized subagents, independent context windows, and delegation.
- **Claude Code — Skills and Subagents**: The distinction between skills and subagents.
- **Claude Agent SDK — Subagents**: Context isolation, parallelization, and specialized instructions.

### Architecture Documentation

- **Martin Fowler — Architecture Decision Record**: ADRs, historical decisions, superseding records.
- **Microsoft — Architecture Decision Records**: When to create ADRs and how to maintain them.
- **Architectural Decision Records**: Community repository of ADR practices and templates.
- **Diátaxis**: A documentation framework: tutorials, how-to guides, reference, and explanation.

### Agent Evaluation

- **Anthropic — Demystifying Evals for AI Agents**: Targeted evaluation of real behaviors over aggregate benchmarks.
- **LangChain — How We Build Evals for Deep Agents**: Behavior-focused evaluation and iterative improvement.

### How the References Map to This Repository

| Concept | Main References |
|---|---|
| Agent workflows | Anthropic — Building Effective Agents |
| Multi-agent orchestration | Anthropic + LangChain |
| Context isolation | LangChain + Claude Code |
| Context engineering | LangChain |
| Filesystem as externalized context | LangChain |
| Specialized subagents | LangChain + Claude Code |
| Independent validation | Anthropic Evals + evaluator patterns |
| Architecture documentation | Fowler + Microsoft |
| Architecture decisions | ADR |
| Documentation organization | Diátaxis |
| Persistent feature artifacts | Context engineering + filesystem patterns |

---

## License

MIT. See [LICENSE](LICENSE).

## Contributing

See [CONTRIBUTING.md](CONTRIBUTING.md).