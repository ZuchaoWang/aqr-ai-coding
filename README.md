# aqr-ai-coding

Custom AI-coding skills and agents — three pattern skills (code patterns, design system, doc patterns), a presentation style skill, one cross-cutting working-principles skill, one manual-only cleanup skill, and one visual inspection subagent. Source repository — install by copying the relevant directory into the host's skills or agents location (user or project scope), then point the agent at it from that project's instructions file. Universal-floor items typically go at user scope; taste-based items at project scope, only in projects that want them.

## Skills

- `aqr-code-pattern` — Universal code quality principles plus opinionated repo layout defaults, frontend architecture patterns, and stack style defaults for code, tests, and notebooks.
- `aqr-design-system` — Applying an existing design system: designing pure HTML+CSS mockups, building new UI, restyling pages, or auditing design consistency.
- `aqr-doc-pattern` — The recommended docs layout (one blueprint for code repos — system and library; noncode projects are out of scope) plus content criteria for common documentation types and markdown style defaults.
- `aqr-presentation` — Opinionated PowerPoint style defaults for decks.
- `aqr-project-principle` — Working principles that define the quality bar for project work: what good work looks like, not a fixed workflow.
- `aqr-cleanup` — Manual-only cleanup pass over a code project. Thorough mode checks code/doc consistency, addresses code problems and quality risks, flags design-level complexity, and verifies fixes; quick mode fixes only typos, stale references, and simple inconsistencies.

The five auto-invocable skills are doc only — no executable code. `aqr-cleanup` is manual-only (invoked by name).

## Agents

Read-only or scoped subagents with focused capabilities. Installed into the host's agents location; invoked by the host agent when their `description` matches the task.

- `visual` — General-purpose agent with vision: reads and edits code and can see screenshots, images, SVG/PNG, and rendered PDFs. Use for any task where pixels matter - investigating rendering bugs, verifying UI, reading diagrams, or repairing what it finds. Returns `CLARIFY` when context is insufficient, otherwise completes the task like any general agent. The vision-capable model is configured externally, not in the agent file.

## Layout

```
skills/<skill-name>/
  SKILL.md              # skill definition
  reference/            # explanatory docs loaded on demand
agents/<agent-name>.md  # subagent definition
configs/                # opencode global config, managed here and copied into ~/.config/opencode/
```

## Installing

Copy the item verbatim into the host's location:

```
# Claude Code
~/.claude/skills/<skill-name>/                # user scope
<project>/.claude/skills/<skill-name>/        # project scope
<project>/.claude/agents/<agent-name>.md      # agents, project scope

# opencode (global config under ~/.config/opencode/, not ~/.opencode/)
~/.config/opencode/skills/<skill-name>/       # user scope
<project>/.opencode/skills/<skill-name>/      # project scope
~/.config/opencode/agents/<agent-name>.md     # agents, user scope
<project>/.opencode/agents/<agent-name>.md    # agents, project scope
```

The host reads each file's frontmatter to decide when to invoke; nothing else needs configuration. After changing this repo, re-copy changed items into `~/.config/opencode/` — the live config holds copies, not symlinks.

## Editing

Match the conventions of existing files: markdown starts with a `#` title, reference docs use numbered headings and `-` bullets with no `---` separators. Verify changes by inspection — the file tree is intact, every `SKILL.md` and agent file has YAML frontmatter (two `---` lines), no file starts with a `Status:` header, and every reference listed in a `SKILL.md` exists. Keep the skill split non-overlapping when adding content.

## References

External sources adapted by these skills:

- [Documenting Architecture Decisions](https://cognitect.com/blog/2011/11/15/documenting-architecture-decisions) (M. Nygard, 2011) — the Architecture Decision Record format adapted in `aqr-doc-pattern`'s decision-record criteria.
- [architecture-decision-record](https://github.com/joelparkerhenderson/architecture-decision-record) (J. P. Henderson) — ADR templates and writing guidance.
- [Presentational and Container Components](https://medium.com/@dan_abramov/smart-and-dumb-components-7ca2f9a7c7d0) (D. Abramov) — the presentational/container distinction adapted in `aqr-code-pattern/reference/frontend/architecture.md` §1.3.
- [Software Architecture skill](https://github.com/davila7/claude-code-templates/blob/main/cli-tool/components/skills/development/software-architecture/SKILL.md) (claude-code-templates) — the library-first / NIH guidance and code-quality rules adapted in `aqr-code-pattern`.
