---
name: aqr-project-principle
description: Use when doing any non-trivial work on a project — making changes, writing or updating docs, investigating. Defines the working principles and quality standards the work should meet.
disable-model-invocation: false
---

# aqr-project-principle

These principles describe **what good work looks like**, not a fixed workflow — the agent may choose any execution strategy that satisfies them.

## 1. Clarify Before Committing

Before making significant semantic or architectural changes:

- clarify ambiguous requirements
- surface important assumptions and trade-offs
- confirm no further requirements are pending
- obtain agreement on the spec or plan

Once clarified, announce that autonomous execution is beginning. Interrupt only for unforeseen blockers or decisions that materially affect the solution.

## 2. Keep Documentation

Documentation is the project's memory.

- record long-lived knowledge: architecture decisions, interface contracts, module responsibilities
- follow documented decisions unless intentionally changing them
- keep documents concise, with only information worth reviewing
- exclude temporary reasoning or planning unless it provides long-term value

## 3. Search Before Solving

Search for an existing answer before working a hard problem out from scratch. For any hard problem or missing information — an unfamiliar algorithm, an unknown concept, details in 3rd party library API — use a found solution rather than reinventing it. Work through these sources in order:

1. official documentation
2. broader discussions (articles, Q&A, issues)
3. library source code, as a last resort
4. if it still does not solve, surface the blocker, skip the subtask and report it

Do not reverse-engineer long minified dependency code or a large dependency repo unless explicitly asked to.

## 4. Verify Before Completion

Before considering work complete:

- run appropriate tests, including visual tests if the project defines them
- verify documentation consistency
- honestly report remaining limitations and uncertainties

Never claim verification that was not actually performed.
