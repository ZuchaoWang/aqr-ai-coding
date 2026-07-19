# aqr-ai-coding

Custom AI-coding skills and agents — two universal skills (an engineering and working floor), three opinionated skills (doc and style taste), one manual-only cleanup skill, and one read-only visual inspection subagent. Source repository — install by copying or symlinking the relevant directory into a project's `.claude/skills/` or `.claude/agents/` folder, then point the agent at it from that project's `CLAUDE.md`. Install per project only where wanted, not at user scope.

## Skills

**Universal** — engineering and working floor:

- `aqr-code-criteria` — Universal code quality principles that apply regardless of language or stack.
- `aqr-project-principle` — Working principles that define the quality bar for project work: what good work looks like, not a fixed workflow.
- `aqr-cleanup` — Manual-only cleanup pass: checks code/doc consistency, refactors code problems, verifies the refactor does not break behavior.

**Opinionated** — taste-based choices a project opts into:

- `aqr-doc-blueprint` — Lay out or compare a project's docs against the recommended `docs/` tree.
- `aqr-doc-content` — Content criteria for common documentation types (design, project, research, dataset).
- `aqr-style-rules` — Opinionated, stack-specific style defaults for code, tests, notebooks, presentations, and doc formatting.

The five auto-invocable skills are doc only — no executable code. `aqr-cleanup` is manual-only (invoked by name).

## Agents

Read-only subagents that perform focused inspection tasks. Installed into a project's `.claude/agents/` folder; invoked by the host agent when their `description` matches the task.

- `visual-inspection` — Visually verifies completed frontend work using browser screenshots at desktop and mobile viewport sizes. Read-only; returns PASS or FAIL with per-issue detail.

## Layout

```
skills/<skill-name>/
  SKILL.md              # skill definition
  reference/            # explanatory docs loaded on demand
agents/<agent-name>.md  # subagent definition
```

See `CLAUDE.md` for editing and installation details.

## References

External sources adapted by `aqr-doc-content` and `aqr-code-criteria`:

- [Documenting Architecture Decisions](https://cognitect.com/blog/2011/11/15/documenting-architecture-decisions) (M. Nygard, 2011) — the Architecture Decision Record format adapted in `reference/design.md` §2.3.
- [architecture-decision-record](https://github.com/joelparkerhenderson/architecture-decision-record) (J. P. Henderson) — ADR templates and writing guidance.
- [Presentational and Container Components](https://medium.com/@dan_abramov/smart-and-dumb-components-7ca2f9a7c7d0) (D. Abramov) — the presentational/container distinction adapted in `reference/design.md` §2.2.
- [Software Architecture skill](https://github.com/davila7/claude-code-templates/blob/main/cli-tool/components/skills/development/software-architecture/SKILL.md) (claude-code-templates) — the library-first / NIH guidance and code-quality rules adapted in `aqr-code-criteria` and `reference/design.md` §2.1.
