# Coding-Agent Prompt & Skill Collection

A collection of prompts and skills that implement a **documentation-first workflow** for building software features with coding agents and subagents.

This repository does not define a product or a framework. It is a toolkit of ready-to-use **agent definitions**, **documentation templates**, and **orchestration skills**. You copy the prompts and skills into your own project.

The underlying workflow applies **agent orchestration, context engineering, feature specification, architecture documentation, and independent validation**.

The goal is not to maximize the number of agents.

The goal is to give each agent **the right responsibility, the right context, and the right handoff**.

The content is harness-agnostic. It works in Claude Code, Codex, pi, and any agent that supports subagents.

---

## What Is in This Repository

| Directory | Content | Purpose |
|---|---|---|
| `src/agents/` | 8 subagent prompt definitions | Define each specialized role: what it reads, produces, and must not do |
| `src/documentation/` | 8 documentation prompt templates | Generate PRD, roadmap, architecture, and decisions documents |
| `src/skills/` | 4 orchestration skills | Drive the root agent through the workflow stages |

---

## How to Use

1. Copy the prompts from `src/agents/` into your agent-subagent definition directory. Claude Code uses `.claude/agents/`. Other tools use their own equivalent.
2. Copy the templates from `src/documentation/` into your documentation prompts.
3. Copy the skills from `src/skills/` into your skill directory.
4. Adapt the model names in the agent definitions to the models you use.

The workflow is model-agnostic. Each project decides which model runs each role.

## Install the Skills with `npx skills`

The orchestration skills in `src/skills/` can be installed directly into any project (or globally) with the [Vercel agent-skills CLI](https://github.com/vercel-labs/agent-skills):

```sh
# Install all skills from this repository into the current project
npx skills add oscarprdev/my-prompts

# Install globally (available in all projects)
npx skills add oscarprdev/my-prompts -g

# Choose which skills and which agents to install
npx skills add oscarprdev/my-prompts -s '*' -a '*'
```

Notes:

- Only the `SKILL.md` files under `src/skills/` are installed. The `agents/` and `documentation/` directories are ignored by the CLI.
- Verify what would be installed before installing:

  ```sh
  npx skills add oscarprdev/my-prompts -l
  ```

- The repository must be public for others to install it.

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

## The Underlying Workflow

The prompts and skills implement a single workflow. Every feature follows a controlled lifecycle:

```text
INTAKE → EXPLORE → DESIGN → DESIGN REVIEW → BUILD → VALIDATE → DOCUMENT → DONE
```

A stage may repeat when a later stage discovers a problem.

The root agent acts as the **orchestrator**. Specialized subagents perform the actual work.

### Agent Roles

| Role | Purpose |
|---|---|
| Explorer | Investigate the existing codebase |
| Designer | Produce the implementation design |
| Design Reviewer | Independently challenge the design |
| Builder | Implement the approved design |
| Validator | Independently verify the implementation |
| Debugger | Diagnose validation failures |
| Reviewer | Review changes against evidence |
| Documenter | Update durable architectural knowledge |

When validation fails, the workflow does not restart from the beginning. The failure is diagnosed and routed to the stage responsible for its root cause.

---

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

## Principles

The prompts enforce these design principles:

1. **Minimize context, not information.** Keep information as artifacts. Inject it only when needed.
2. **Parallelize research, not decisions.** Investigation can run in parallel. Ownership stays explicit.
3. **One owner per artifact.** One agent owns each artifact. Others may read it but do not silently rewrite it.
4. **Prefer deterministic workflows.** Explicit stages and gates work well for well-defined processes.
5. **Fail locally.** A failure returns to the stage responsible for its root cause.
6. **Keep durable knowledge outside the conversation.** The filesystem becomes part of the agent's working memory.

Additional rules:

- **Independent validation.** The agent that builds a feature does not validate its own work. Reviewers receive an independent context.
- **Capability-to-task model matching.** Exploration can run on a cheap, fast model. Design and implementation benefit from a strong reasoning model. Review should use an independent model.

---

## References

The prompts are a synthesis of established work on agent orchestration, context engineering, software architecture, and agent evaluation.

### Agent Workflows

- **Anthropic — Building Effective Agents**: Agent architectures, including sequential workflows, parallelization, routing, orchestrator-worker patterns, and evaluator-optimizer systems.
- **Anthropic — Building Effective AI Agents: Architecture Patterns**: Single-agent systems, multi-agent orchestration, context management, modularity, and Skills.
- **LangChain — How and When to Build Multi-Agent Systems**: When multi-agent architectures are useful and why context engineering matters.
- **LangChain — Subagents**: Supervisor/subagent architectures, input context, output contracts, routing, and context isolation.

### Context Engineering

- **LangChain — Context Engineering for Agents**: The strategies of writing, selecting, compressing, and isolating context.
- **LangChain — How Agents Can Use Filesystems for Context Engineering**: Using filesystems to offload context and preserve intermediate work.
- **LangChain — Context Management for Deep Agents**: Compression, filesystem offloading, summarization, and long-running workflows.
- **LangChain — Using Skills with Deep Agents**: Skills as reusable instruction packages.

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

---

## License

MIT. See [LICENSE](LICENSE).

## Contributing

See [CONTRIBUTING.md](CONTRIBUTING.md).