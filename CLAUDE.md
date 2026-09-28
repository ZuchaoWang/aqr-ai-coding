# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Repository purpose

This repository holds custom AI-coding skills and agents. Skills are reusable principles, criteria, and workflows read and applied by the host agent. Agents are focused subagents dispatched by the host for specific tasks. Each skill is a self-contained directory with a `SKILL.md`, optionally with a `reference/` tree of explanatory docs. Each agent is a single markdown file with YAML frontmatter. Skills and agents are designed to compose with execution methodology skills (e.g. Superpowers) but are not tied to any specific one.

All skills in this repo are **doc only**. No skill ships executable code. Their effect comes from being read and applied. Agents may ship a small amount of inline guidance but no executable code either.

## Layout

```
skills/
  <skill-name>/
    SKILL.md                  # skill definition: YAML frontmatter + body
    reference/                # explanatory docs loaded on demand; subfolders allowed
agents/<agent-name>.md        # subagent definition: YAML frontmatter + body
```

Each `SKILL.md` starts with YAML frontmatter:

```yaml
---
name: <skill-name>
description: <one-paragraph description used by the host agent to decide invocation>
disable-model-invocation: <true | false | omit>
---
```

`disable-model-invocation: true` marks a skill manual-only (the host agent will not auto-invoke it based on context; the user must name it explicitly). Omit the field or set it `false` for auto-invocable skills.

Each agent file uses the Claude Code subagent frontmatter:

```yaml
---
description: <one-line description used by the host to decide dispatch>
mode: subagent
permission:
  edit: <allow | deny>
---
```

`mode: subagent` runs the agent as an isolated subagent dispatched by the host. `permission.edit: deny` makes it read-only.

## Skills currently in this repo

- `aqr-code-pattern` — Universal code quality and design principles plus opinionated frontend architecture patterns, design-system application guidance, and stack style defaults for code, tests, and notebooks. Applied when writing or reviewing code, UI, or implementation design docs; criteria are not copied into the project.
- `aqr-doc-pattern` — The recommended docs layout (one blueprint for code repos — system and library; noncode projects are out of scope) plus content criteria for every standard doc type (implementation design, UI design, project, research, dataset) and markdown formatting defaults. Applied when laying out, writing, or reviewing docs; criteria are not copied into the project.
- `aqr-presentation` — Opinionated PowerPoint style defaults for decks. Applied when creating or editing decks; not copied into the project.
- `aqr-project-principle` — Working principles that define the quality bar for project work: what good work looks like, not a fixed workflow. Applied during work; not copied into the project.
- `aqr-cleanup` — Manual-only cleanup pass over a code project. Thorough mode: checks code/doc consistency, addresses code problems and quality risks, flags design-level complexity, and verifies fixes. Quick mode: fixes only typos, stale references, and simple inconsistencies. Invoked by name only.

The four auto-invocable skills may be invoked by the host based on context, and the user can also name one directly via slash command. `aqr-cleanup` is manual-only.

The split is deliberate: code quality plus frontend architecture plus design-system application plus stack styles (`aqr-code-pattern`), docs layout plus content criteria plus doc formatting (`aqr-doc-pattern`), deck style (`aqr-presentation`), working principles (`aqr-project-principle`), and an on-demand cleanup pass (`aqr-cleanup`) are independent concerns. A project can adopt any combination.

## Agents currently in this repo

- `visual` — General-purpose agent with vision. Dispatched for any task where pixels matter: investigating rendering bugs, verifying UI against a spec or reference image, reading diagrams, charts, or screenshots, or repairing what it finds. Reads and edits code and can see screenshots, images, SVG/PNG, and rendered PDFs. Returns `CLARIFY` when the provided context is insufficient, asking the caller for a complete re-brief. The vision-capable model is configured externally (e.g. `agent.visual.model`), not in the agent file.

## Conventions inside skill and agent files

- Markdown files start with a top-level heading (`# Title`).
- Reference docs use numbered headings and `-` bullets. No `---` horizontal separators.
- `SKILL.md` files use YAML frontmatter — they are skill definitions, distinct from project docs.
- Agent files use the subagent frontmatter shown above.

## Editing skills

When editing a skill:

1. Read the existing `SKILL.md` first to understand the skill's scope and invocation policy.
2. Match the conventions of existing files in the same skill (numbered headings, `-` bullets, no `---`, no `Status:` header).
3. Cross-check the split: `aqr-code-pattern` covers code quality, frontend architecture, design-system application, and stack styles; `aqr-doc-pattern` covers docs layout, doc content, and doc formatting; `aqr-presentation` covers deck style only; `aqr-project-principle` covers working standards only; `aqr-cleanup` is an on-demand action, not a standing criterion. None of the standing criteria should overlap.
4. If a reference change affects invocation behavior, update `SKILL.md` accordingly.

## Editing agents

When editing an agent:

1. Read the existing agent file first to understand its scope, `mode`, and `permission`.
2. Keep the `description` accurate — the host uses it to decide when to dispatch the agent.
3. Keep the body focused on what the agent must do and what it must return. Do not duplicate skill content; reference a skill by name if the agent should apply it.

## Installing skills and agents

These are opt-in. Skills that state a universal floor are typically installed at user scope; taste-based skills at project scope, only in projects that want them. Copy or symlink each item verbatim into the host's location.

Claude Code:

```
~/.claude/skills/<skill-name>/               # user scope
<project>/.claude/skills/<skill-name>/        # project scope
<project>/.claude/agents/<agent-name>.md      # agents, project scope
```

opencode (global config lives under `~/.config/opencode/`, not `~/.opencode/`):

```
~/.config/opencode/skills/<skill-name>/       # user scope
<project>/.opencode/skills/<skill-name>/      # project scope
~/.config/opencode/agents/<agent-name>.md     # agents, user scope
<project>/.opencode/agents/<agent-name>.md    # agents, project scope
```

The host reads the frontmatter to decide when to invoke; nothing else needs configuration. In opencode, an agent's model and permissions are typically set in `opencode.json` under `agent.<name>` rather than in the agent file.

Skill auto-invocation is unreliable on its own, so the adopting project should also add a directive to its own `CLAUDE.md` telling the agent to use them. Example:

```md
For software-development work, use the AQR skills under `.claude/skills/` and invoke the one that matches the task (do not invoke all of them):

- `aqr-project-principle` — any non-trivial work (the quality bar for the work)
- `aqr-code-pattern` — writing or reviewing source code or implementation design docs; frontend (React) code; UI against a design system; stack style defaults
- `aqr-doc-pattern` — laying out or auditing the `docs/` tree; writing or reviewing docs (implementation design, UI design, project, research, dataset); markdown style
- `aqr-presentation` — creating or editing PowerPoint decks

`aqr-cleanup` and the agents under `.claude/agents/` are manual — invoke them by name when needed.
```

## Verifying changes

There is no test suite. Verification is by inspection:

- `find skills agents -type f | sort` — check the file tree is intact.
- `grep -rn "^Status:" skills/ agents/` — should return nothing. Markdown files start with a `#` title, not a `Status:` header.
- `grep -n "^---$" skills/*/SKILL.md` — confirm every SKILL.md has YAML frontmatter (two `---` lines).
- `grep -n "^---$" agents/*.md` — confirm every agent file has YAML frontmatter (two `---` lines).
- Read each `SKILL.md` and agent file end-to-end before considering it ready.
