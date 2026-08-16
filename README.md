# Agent Workflow

A documentation-first workflow for building reliable software features with coding agents and specialized subagents.

This repository defines a practical approach to **agent orchestration, context engineering, feature specification, architecture documentation, and independent validation**.

The goal is not to maximize the number of agents.

The goal is to give each agent **the right responsibility, the right context, and the right handoff**.

---

## Philosophy

The workflow is built around a few principles:

* **Specialize agents instead of making one agent do everything.**
* **Isolate context instead of continuously growing a single conversation.**
* **Use artifacts as handoffs instead of passing entire conversations between agents.**
* **Separate planning, implementation, and validation.**
* **Keep architectural knowledge persistent and feature knowledge traceable.**
* **Use the simplest workflow that reliably solves the task.**
* **Treat the repository as the source of truth for current implementation.**

This approach combines ideas from agent orchestration, context engineering, software architecture documentation, and established engineering practices.

---

## Workflow

Every feature follows a controlled lifecycle:

```text
INTAKE → EXPLORE → DESIGN → DESIGN REVIEW → BUILD → VALIDATE → DOCUMENT → DONE
```

A stage may repeat when a later stage discovers a problem.

The root agent acts as the **orchestrator**. Specialized subagents perform the actual work.

Typical responsibilities are:

* **Explorer** — understand the existing codebase.
* **Designer** — produce the implementation design.
* **Design Reviewer** — independently challenge the design.
* **Builder** — implement the approved design.
* **Validator** — independently verify the implementation.
* **Debugger** — diagnose validation failures.
* **Documentation Agent** — update durable architectural knowledge when necessary.

---

## Artifact-Driven Handoffs

Agents do not pass their complete conversations to each other.

Instead, each stage produces a persistent artifact:

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

* Context isolation
* Explicit contracts
* Persistent state
* Traceable decisions
* Smaller prompts
* Easier retries
* Better reproducibility

Large or noisy information can remain outside the active model context and be loaded only when required.

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

### Product

Describes **what the product is trying to achieve**.

### Architecture

Describes **how the system is structured**.

### Decisions

Describes **why important architectural decisions were made**.

### Conventions

Describes **established engineering practices**.

### Specs

Describes **how a specific feature is being researched, designed, implemented, and validated**.

The code remains the source of truth for current implementation.

---

## Context Engineering

A central design principle is that every subagent should receive only the context necessary for its current task.

Instead of:

```text
entire repository
+ entire documentation tree
+ entire conversation
+ previous agent reasoning
```

the workflow prefers:

```text
task
+ relevant artifact
+ relevant documentation
+ repository access
```

This follows the broader idea of **context engineering**: selecting, writing, compressing, and isolating context so the model receives the information needed for the next step without unnecessary context accumulation.

---

## Why Subagents?

Subagents are primarily used for:

* Context isolation
* Specialization
* Independent validation
* Parallel research
* Reducing unnecessary context in the main session

A subagent is not necessarily a more capable model.

Often, its main value is that it gets **a clean context window dedicated to one problem**.

This repository therefore treats subagents as specialized workers rather than independent conversational assistants.

---

## Independent Validation

Generation and evaluation are intentionally separated.

The agent that designs or implements a feature should not automatically be trusted to validate its own work.

For example:

```text
Designer
   ↓
Design Reviewer

Builder
   ↓
Validator
```

Reviewers receive an independent context and are expected to challenge the previous stage rather than simply continue it.

When validation fails, the workflow does not automatically restart from the beginning. The failure is diagnosed and routed to the stage responsible for fixing its root cause.

---

## Architecture Documentation

The repository distinguishes between **current implementation** and **architectural intent**.

The code answers:

> What does the system currently do?

Architecture documentation answers:

> What are the important boundaries, responsibilities, and invariants?

Architecture Decision Records answer:

> Why was this decision made?

This avoids turning documentation into a second, constantly outdated copy of the codebase.

Architectural documentation should focus on durable knowledge that is difficult to infer quickly from implementation details.

---

## Architecture Decision Records

Important architectural decisions are recorded as individual ADRs.

An ADR should capture:

* Context
* Decision
* Alternatives
* Consequences
* Constraints
* Status

Historical decisions should not be rewritten when the architecture changes. A new decision should supersede the previous one.

This preserves the evolution of architectural thinking rather than only documenting the current state.

---

## When to Update Documentation

Not every implementation change requires documentation changes.

Documentation should be updated when a feature changes durable knowledge such as:

* Architectural boundaries
* Domain boundaries
* Component responsibilities
* Important invariants
* Public contracts
* Major data flows
* External integrations
* Architectural decisions
* Established conventions

Normal implementation details, small refactors, tests, and bug fixes should generally remain represented by code and tests.

---

## Model Assignment

The workflow is model-agnostic.

Different stages can use different models depending on their requirements.

A practical strategy is:

```text
Exploration / high-volume research
        ↓
cheap / fast model

Design / implementation
        ↓
strong reasoning model

Review / validation
        ↓
independent model
```

The important principle is **capability-to-task matching**, not using the strongest model everywhere.

Model selection should ultimately be measured using:

* Cost
* Latency
* First-pass success
* Retry rate
* Context usage
* Overall task success

---

## Design Principles

### 1. Minimize context, not information

Do not remove information that matters.

Instead, keep information available as artifacts and inject it only when needed.

### 2. Parallelize research, not decisions

Independent investigation can happen in parallel.

Architectural decisions and implementation ownership should remain explicit.

### 3. One owner per artifact

Each artifact has one responsible agent.

Other agents may read it, but should not silently rewrite it.

### 4. Prefer deterministic workflows where possible

Well-defined engineering processes benefit from explicit stages and gates.

Use dynamic agent behavior when the problem genuinely requires it.

### 5. Fail locally

A validation failure should normally return to the stage responsible for its root cause instead of restarting the entire workflow.

### 6. Keep durable knowledge outside the conversation

Important discoveries, decisions, and validation results should become persistent artifacts.

The filesystem becomes part of the agent's working memory.

---

# References

This workflow is a synthesis of established work on agent orchestration, context engineering, software architecture, documentation, and agent evaluation.

## Agent Workflows

### Anthropic — Building Effective Agents

Foundational work on agent architectures, including sequential workflows, parallelization, routing, orchestrator-worker patterns, and evaluator-optimizer systems.

:contentReference[oaicite:0]{index=0}

### Anthropic — Building Effective AI Agents: Architecture Patterns

More recent guidance covering single-agent systems, multi-agent orchestration, sequential and parallel workflows, evaluator-optimizer patterns, context management, modularity, and Skills.

:contentReference[oaicite:1]{index=1}

### LangChain — How and When to Build Multi-Agent Systems

Discusses when multi-agent architectures are useful, the importance of context engineering, and why heavily parallelizable research tasks are a stronger fit for multi-agent systems than many coding tasks.

:contentReference[oaicite:2]{index=2}

### LangChain — Subagents

Detailed reference for supervisor/subagent architectures, subagent specifications, input context, output contracts, routing, and context isolation.

:contentReference[oaicite:3]{index=3}

---

## Context Engineering

### LangChain — Context Engineering for Agents

A foundational reference for context engineering and the strategies of writing, selecting, compressing, and isolating context.

:contentReference[oaicite:4]{index=4}

### LangChain — How Agents Can Use Filesystems for Context Engineering

Explores using filesystems to offload large context, preserve intermediate work, and selectively retrieve information instead of keeping everything in the active conversation.

:contentReference[oaicite:5]{index=5}

### LangChain — Context Management for Deep Agents

Covers context compression, filesystem offloading, summarization, and managing long-running agent workflows.

:contentReference[oaicite:6]{index=6}

### LangChain — Using Skills with Deep Agents

Explores skills as reusable instruction/context packages and their relationship with filesystem-based agent workflows.

:contentReference[oaicite:7]{index=7}

---

## Coding Subagents

### Claude Code — Custom Subagents

Practical documentation for specialized coding subagents, independent context windows, tool restrictions, delegation, and project-level subagent definitions.

:contentReference[oaicite:8]{index=8}

### Claude Code — Skills and Subagents

Useful reference for understanding the distinction between skills and subagents and when context isolation is appropriate.

:contentReference[oaicite:9]{index=9}

### Claude Agent SDK — Subagents

Explains using subagents to isolate context, parallelize focused work, and apply specialized instructions.

:contentReference[oaicite:10]{index=10}

---

## Architecture Documentation

### Martin Fowler — Architecture Decision Record

Defines ADRs as short records of individual architectural decisions, including context, decision, and consequences. It also recommends preserving historical decisions and creating superseding ADRs rather than rewriting them.

:contentReference[oaicite:11]{index=11}

### Microsoft — Architecture Decision Records

Guidance on when to create ADRs, what they should contain, and how to maintain architectural decision history.

:contentReference[oaicite:12]{index=12}

### Architectural Decision Records

Community repository containing ADR practices, formats, templates, and additional references.

:contentReference[oaicite:13]{index=13}

### Diátaxis

A documentation framework that separates documentation into tutorials, how-to guides, reference, and explanation.

:contentReference[oaicite:14]{index=14}

---

## Agent Evaluation

### Anthropic — Demystifying Evals for AI Agents

Discusses evaluation strategies for autonomous agents, emphasizing targeted evaluation of real behaviors rather than relying only on aggregate benchmarks.

:contentReference[oaicite:15]{index=15}

### LangChain — How We Build Evals for Deep Agents

Describes behavior-focused evaluation, targeted eval suites, trace analysis, and using evaluations to iteratively improve agent workflows.

:contentReference[oaicite:16]{index=16}

---

## How These References Map to This Repository

The repository combines these ideas into a concrete workflow:

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

The exact combination used in this repository — feature artifacts, specialized coding agents, explicit handoffs, isolated validation, persistent specifications, and architecture documentation — is the practical architecture proposed here, built on top of these established principles.
