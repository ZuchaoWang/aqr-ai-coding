---
name: aqr-doc-pattern
description: The recommended docs layout plus content criteria for common documentation types and markdown formatting defaults. Use when laying out a project's docs or deciding where a doc belongs; when writing or reviewing any project doc (implementation design, UI design, project, research, dataset); or when applying this repo's markdown style defaults.
disable-model-invocation: false
---

# aqr-doc-pattern

Three layers: the blueprint says what docs a project should have and where (layout only, not content); the templates define what good content looks like for each doc type; the style section below sets markdown formatting defaults. When writing or reviewing a doc, load the matching template and apply the sections that fit.

## Blueprint: docs layout

Every project — code or not — starts from this root layout:

```
README.md                # what the project is and how to start 
CLAUDE.md                # agent instructions and toolchain specifics; also a brief top-level directory map
docs/                    # the detailed documentation, including background, research, design, implementation, etc.
```

For code project (systems and libraries), the recommended file structure under `docs/` folder can be found in `reference/blueprint.md`. Use it to bootstrap a new docs tree or to audit an existing one for drift. The blueprint is a baseline, not a prescription — do not impose it on a project that has diverged; report drift instead. We do not cover noncode projects because docs are their primary output and the structure varies too much to prescribe.

## Templates: content criteria by doc type

Groups follow the blueprint's docs tree (`project/`, `research/`, `implementation/`, `data/`; research and data share one group here); rows within a group follow the blueprint's file order.

### Project docs

| Doc type | What it covers | Reference |
| - | - | - |
| Mission | Goal, problem statement, scope, stakeholders | `reference/templates/mission.md` |
| Status | Current stage, next tasks, decisions log | `reference/templates/status.md` |
| Usage scenarios | Concrete situations the project must handle | `reference/templates/usage_scenarios.md` |
| Concepts | Active domain vocabulary | `reference/templates/concepts.md` |
| UI design | Page structure and navigation, per-page interaction, chart visualization encoding | `reference/templates/ui_design.md` |
| API design | Project level public API: signatures and semantics, usage examples, decisions | `reference/templates/api_design.md` |

### Implementation docs

| Doc type | What it covers | Reference |
| - | - | - |
| Implementation design (system or module) | How a predefined interface is implemented: summary, data and state, decomposition, data and control flow, nontrivial implementation hints — frontend app variations included | `reference/templates/implementation_design.md` |
| Tech stack | Languages, frameworks, toolchain, rationale | `reference/templates/tech_stack.md` |

### Other docs

| Doc type | What it covers | Reference |
| - | - | - |
| Research docs | Domain background, related-work comparison, design-option brainstorm | `reference/templates/research.md` |
| Dataset doc | Data acquisition, processing and description | `reference/templates/dataset.md` |

## Style: markdown formatting

Opinionated formatting defaults, applied unless the project records different choices:

- Place a summary paragraph after the title and before subsections in technical docs.
- Use numbered headings (`## 1. Overview`, `### 1.1 Motivation`) so cross-references stay stable.
- Do not use `---` horizontal separators; restructure instead. Use `-` for list items, not `*`.
- For two-column tables, convert to key: value lists instead.
- Render diagrams as Mermaid fenced blocks (` ```mermaid `), not ASCII art or image files, so they render inline and stay editable as text.
- Enforce these in `.markdownlint.json` at the repo root, disabling any rule that conflicts with this style (for example, the horizontal-rule rule, since separators are disallowed, and any line-length rule).
