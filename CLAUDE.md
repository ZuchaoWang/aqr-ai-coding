# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Repository purpose

This repository holds custom AI-coding skills and agents. Skills are reusable principles, criteria, and workflows read and applied by the host agent. Agents are focused subagents dispatched by the host for specific tasks. Each skill is a self-contained directory with a `SKILL.md`, optionally with a `reference/` tree of explanatory docs. Each agent is a single markdown file with YAML frontmatter. Skills and agents are designed to compose with execution methodology skills (e.g. Superpowers) but are not tied to any specific one.

All skills in this repo are **doc only**. No skill ships executable code. Their effect comes from being read and applied. Agents may ship a small amount of inline guidance but no executable code either.

## Layout

```
skills/<skill-name>/
  SKILL.md              # skill definition: YAML frontmatter + body
  reference/            # explanatory docs loaded on demand
agents/<agent-name>.md  # subagent definition: YAML frontmatter + body
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

**Universal** — engineering and working floor, not taste:

- `aqr-code-criteria` — Universal code quality principles that apply regardless of language or stack. Applied when writing or reviewing code; criteria are not copied into the project.
- `aqr-project-principle` — Working principles that define the quality bar for project work: what good work looks like, not a fixed workflow. Applied during work; not copied into the project.
- `aqr-cleanup` — Manual-only cleanup pass over an existing project: checks code/doc consistency, refactors code problems, verifies the refactor does not break behavior. Invoked by name only.

**Opinionated** — taste-based choices:

- `aqr-doc-blueprint` — Reference for the recommended docs layout: the `docs/` tree plus the root entry points that route into it. Describes what docs a project should have and where; not how to write them. Does not mention issues or workflow.
- `aqr-doc-content` — Content criteria for every standard doc type in the blueprint layout (design, project, research, dataset). Applied when writing or reviewing docs; criteria are not copied into the project.
- `aqr-style-rules` — Opinionated, stack-specific style defaults (code, tests, notebooks, presentations, doc formatting) layered on top of the code and doc criteria. Applied when writing or reviewing code, tests, notebooks, or decks; not copied into the project.

The five auto-invocable skills may be invoked by the host based on context, and the user can also name one directly via slash command. `aqr-cleanup` is manual-only.

The split is deliberate: docs layout (`aqr-doc-blueprint`), doc content (`aqr-doc-content`), code content (`aqr-code-criteria`), working principles (`aqr-project-principle`), opinionated style taste (`aqr-style-rules`), and an on-demand cleanup pass (`aqr-cleanup`) are independent concerns. A project can adopt any combination.

## Agents currently in this repo

- `visual-inspection` — Read-only visual frontend inspection. Dispatched after frontend work to verify the rendered result with browser screenshots at desktop and mobile viewport sizes. Returns PASS or FAIL with per-issue detail. Cannot edit files.

## Conventions inside skill and agent files

- Markdown files start with a top-level heading (`# Title`).
- Reference docs use numbered headings and `-` bullets. No `---` horizontal separators.
- `SKILL.md` files use YAML frontmatter — they are skill definitions, distinct from project docs.
- Agent files use the subagent frontmatter shown above.

## Editing skills

When editing a skill:

1. Read the existing `SKILL.md` first to understand the skill's scope and invocation policy.
2. Match the conventions of existing files in the same skill (numbered headings, `-` bullets, no `---`, no `Status:` header).
3. Cross-check the split: `aqr-doc-blueprint` covers docs layout only; `aqr-doc-content` covers doc content quality only; `aqr-code-criteria` covers code content quality only; `aqr-project-principle` covers working standards only; `aqr-style-rules` covers opinionated style taste on top of the code and doc criteria only; `aqr-cleanup` is an on-demand action, not a standing criterion. None of the standing criteria should overlap.
4. If a reference change affects invocation behavior, update `SKILL.md` accordingly.

## Editing agents

When editing an agent:

1. Read the existing agent file first to understand its scope, `mode`, and `permission`.
2. Keep the `description` accurate — the host uses it to decide when to dispatch the agent.
3. Keep the body focused on what the agent must do and what it must return. Do not duplicate skill content; reference a skill by name if the agent should apply it.

## Installing skills and agents

These are opt-in. Install them **per project** — not at user scope — only in projects that want them. For Claude Code, copy or symlink each item into the project's corresponding folder:

```
<project>/.claude/skills/<skill-name>/
<project>/.claude/agents/<agent-name>.md
```

Copy the directory or file verbatim. The host agent reads the frontmatter to decide when to invoke; nothing else needs configuration.

Skill auto-invocation is unreliable on its own, so the adopting project should also add a directive to its own `CLAUDE.md` telling the agent to use them. Example:

```md
For software-development work, use the AQR skills under `.claude/skills/` and invoke the one that matches the task (do not invoke all of them):

- `aqr-project-principle` — any non-trivial work (the quality bar for the work)
- `aqr-code-criteria` — writing or reviewing source code
- `aqr-doc-content` — writing or reviewing docs (design, project, research, dataset)
- `aqr-doc-blueprint` — laying out or auditing the `docs/` tree
- `aqr-style-rules` — formatting and style for code, tests, notebooks, decks, docs

`aqr-cleanup` and the agents under `.claude/agents/` are manual — invoke them by name when needed.
```

## Verifying changes

There is no test suite. Verification is by inspection:

- `find skills agents -type f | sort` — check the file tree is intact.
- `grep -rn "^Status:" skills/ agents/` — should return nothing. Markdown files start with a `#` title, not a `Status:` header.
- `grep -n "^---$" skills/*/SKILL.md` — confirm every SKILL.md has YAML frontmatter (two `---` lines).
- `grep -n "^---$" agents/*.md` — confirm every agent file has YAML frontmatter (two `---` lines).
- Read each `SKILL.md` and agent file end-to-end before considering it ready.
