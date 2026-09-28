# aqr-ai-coding

Custom AI-coding skills and agents — two consolidated skills (code patterns, doc patterns), a presentation style skill, one cross-cutting working-principles skill, one manual-only cleanup skill, and one visual inspection subagent. Source repository — install by copying or symlinking the relevant directory into a project's `.claude/skills/` or `.claude/agents/` folder, then point the agent at it from that project's `CLAUDE.md`. Install per project only where wanted, not at user scope.

## Skills

- `aqr-code-pattern` — Universal code quality principles plus opinionated frontend architecture patterns, design-system application guidance, and stack style defaults for code, tests, and notebooks.
- `aqr-doc-pattern` — The recommended docs layout (one blueprint for code repos — system and library; noncode repos are a simple, free-form exception) plus content criteria for common documentation types and markdown style defaults.
- `aqr-presentation` — Opinionated PowerPoint style defaults for decks.
- `aqr-project-principle` — Working principles that define the quality bar for project work: what good work looks like, not a fixed workflow.
- `aqr-cleanup` — Manual-only cleanup pass over a code project: checks code/doc consistency, refactors code problems, verifies the refactor does not break behavior.

The four auto-invocable skills are doc only — no executable code. `aqr-cleanup` is manual-only (invoked by name).

## Agents

Read-only or scoped subagents with focused capabilities. Installed into a project's `.claude/agents/` folder; invoked by the host agent when their `description` matches the task.

- `visual` — General-purpose agent with vision: reads and edits code and can see screenshots, images, SVG/PNG, and rendered PDFs. Use for any task where pixels matter - investigating rendering bugs, verifying UI, reading diagrams, or repairing what it finds. Returns `CLARIFY` when context is insufficient, otherwise completes the task like any general agent. The vision-capable model is configured externally, not in the agent file.

## Layout

```
skills/<skill-name>/
  SKILL.md              # skill definition
  reference/            # explanatory docs loaded on demand
agents/<agent-name>.md  # subagent definition
```

See `CLAUDE.md` for editing and installation details.

## References

External sources adapted by these skills:

- [Documenting Architecture Decisions](https://cognitect.com/blog/2011/11/15/documenting-architecture-decisions) (M. Nygard, 2011) — the Architecture Decision Record format adapted in `aqr-doc-pattern`'s decision-record criteria.
- [architecture-decision-record](https://github.com/joelparkerhenderson/architecture-decision-record) (J. P. Henderson) — ADR templates and writing guidance.
- [Presentational and Container Components](https://medium.com/@dan_abramov/smart-and-dumb-components-7ca2f9a7c7d0) (D. Abramov) — the presentational/container distinction adapted in `aqr-code-pattern/reference/frontend_patterns.md` §1.3.
- [Software Architecture skill](https://github.com/davila7/claude-code-templates/blob/main/cli-tool/components/skills/development/software-architecture/SKILL.md) (claude-code-templates) — the library-first / NIH guidance and code-quality rules adapted in `aqr-code-pattern`.
