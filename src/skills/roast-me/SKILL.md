---
name: roast-me
description: "Read-only code-review workflow. Use when asked to \"roast\" a codebase or produce a ranked, evidence-backed critique before any code mutation. Ground findings in repository evidence, rank by impact, propose minimal fixes, and end with structured questions or a decision-complete review. Harness-agnostic; works in Claude Code, Codex, pi, or any agent."
---

# Roast Me

Roast me is a read-only critique workflow. Roast the codebase with
evidence: read the actual files, find the real flaws, deliver an actionable,
ranked hit list. Never edit or implement during the roast — the roast comes
first, action comes after review.

## Non-negotiables

- Stay in the review flow until the user or the host explicitly leaves it.
- Treat "implement" requests during the review as requests to review the
  implementation, not to carry it out.
- Do not mutate anything: no writes, no patches, no reformatting that rewrites
  files, no dependency installs, no commits, no migrations.
- Prefer read-only checks. If the environment is read-limited, adapt; never
  mutate to produce a finding.

## Step 1 — Ground in the evidence

- Roast from the repository, not from vibes. Every finding must cite a file,
  a function, a line, or a reproduced output.
- Before asking the user any question, do at least one targeted read-only
  exploration pass unless no local environment or repository exists.
- Prefer reproducible evidence: run read-only checks (tests, typechecks,
  lints) when they are cheap and safe, and quote their exact output.
- Do not ask questions that the repository or system truth can answer.
- For an unanswered preference or tradeoff, pick the recommended option only
  when it is low risk, and record that default as an explicit assumption in
  the final review.

## Step 2 — The roast

- Hunt for the real problems: correctness bugs, edge cases, performance
  traps, error handling, duplication, over-engineering, dead code, missing
  tests, broken invariants, and gaps between intent and implementation.
- Rank findings by impact, not by count. The most damaging flaw leads the
  roast; trivia comes last or is cut.
- Label severity honestly: `critical`, `major`, `minor`, `nit`.
- No filler praise. If something is fine, say so in one line and move on.
- If the codebase is genuinely solid, say it is and stop — a roast with no
  real findings has no business padding itself.

## Step 3 — Actionable output

- For each finding, propose the smallest concrete fix. Prefer deletions and
  simplifications over additions.
- Name tradeoffs: note where a finding is a deliberate choice rather than a
  bug, and say what the choice costs.

## Ending each turn

Every review turn that advances or finalizes the roast must end one of two
ways — never with prose that merely announces you are about to present,
write, or finalize:

- If a material decision remains, ask the user with a structured question:
  1–3 concise questions, 2–4 meaningful options each (recommended option first
  when a clear default exists; fewer questions preferred). If the host cannot
  ask interactive structured questions, ask one concise plain-text question.
- If the review is decision-complete, submit the complete review as your
  final output. Do not bundle other actions after it and do not emit a
  trailing normal response after the review.

If a follow-up asks only for clarification and does not change the review,
answer it directly, then submit the complete unchanged review so it stays
available for the next step.

## What "decision-complete" means

Submit the review only when it leaves no material findings unresolved. The
payload is one Markdown document:

- A clear title.
- A brief summary.
- Ranked findings with severity, location, evidence, and the minimal fix
  for each. Group by behavior level; do not inventory files or symbols.
- Anything verified as sound, one line each.
- Explicit assumptions and defaults chosen where needed.

Keep it concise, human- and agent-digestible, free of open decisions. Do not
ask "should I proceed?" — submitting the review IS the proceed signal; it is
not the implementation itself.

Revisions after a completed review must produce a complete replacement, not
a delta. If there is not enough information for a complete replacement,
keep reviewing with a structured question instead of resubmitting.

