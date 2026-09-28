---
name: aqr-doc-pattern
description: The recommended docs layout plus content criteria for common documentation types and markdown formatting defaults. Use when laying out a project's docs or deciding where a doc belongs; when writing or reviewing any project doc (design, frontend design, UI design, project, research, dataset); or when applying this repo's markdown style defaults.
disable-model-invocation: false
---

# aqr-doc-pattern

Three layers: the blueprint says what docs a project should have and where (layout only, not content); the templates define what good content looks like for each doc type; the style section below sets markdown formatting defaults. When writing or reviewing a doc, load the matching template and apply the sections that fit.

## Blueprint: docs layout

Root `README.md` and `CLAUDE.md` route readers and agents into the docs. `docs/index.md` is the ground truth for a project's docs — the map of what actually exists. One recommended tree covers code projects — deployed systems and libraries — with optional files marked: `reference/blueprint.md`. Use it to bootstrap a new docs tree or to audit an existing one for drift. The blueprint is a baseline, not a prescription — do not impose it on a project that has diverged; report drift instead. Noncode repos are an exception: their docs are the primary output and the structure varies too much to prescribe — keep it simple, do not impose the blueprint.

## Templates: content criteria by doc type

### Design docs

| Doc type | What it covers | Reference |
| - | - | - |
| Design doc (system or module) | Public interface, basic design, nontrivial implementation hints | `reference/templates/design.md` |
| Tech stack | Languages, frameworks, toolchain, rationale | `reference/templates/tech_stack.md` |
| Frontend design doc | Component tree, global state, data flow — for designs describing a frontend | `reference/templates/frontend_design.md` |
| UI design doc | Page structure and navigation, per-page interaction, chart visualization encoding | `reference/templates/ui_design.md` |

### Project docs

| Doc type | What it covers | Reference |
| - | - | - |
| Mission | Goal, problem statement, scope, stakeholders | `reference/templates/mission.md` |
| Usage scenarios | Concrete situations the project must handle | `reference/templates/usage_scenarios.md` |
| Roadmap | Vision, milestones, decisions log | `reference/templates/roadmap.md` |
| Concepts | Active domain vocabulary | `reference/templates/concepts.md` |
| API design | Public API: signatures and semantics, usage examples, decisions | `reference/templates/api_design.md` |

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
