---
name: aqr-doc-pattern
description: The recommended docs layout plus content criteria for common documentation types and markdown formatting defaults. Use when laying out a project's docs or deciding where a doc belongs; when writing or reviewing any project doc (system design, frontend design, UI design, project, research, dataset); or when applying this repo's markdown style defaults.
disable-model-invocation: false
---

# aqr-doc-pattern

Three layers: the blueprint says what docs a project should have and where (layout only, not content); the templates define what good content looks like for each doc type; the styles set markdown formatting defaults. When writing or reviewing a doc, load the matching reference and apply the sections that fit.

## Blueprint: docs layout by repo type

Two root files route readers and agents into the docs:

```
README.md                # what the project is and how to start; points at docs/index.md
CLAUDE.md                # agent instructions and toolchain specifics; also a brief top-level directory map
```

`docs/index.md` is the ground truth for a project's docs — the map of what actually exists. The recommended tree under it depends on the repo type. Use the matching reference to bootstrap a new docs tree or to audit an existing one for drift. The reference is a baseline, not a prescription — do not impose it on a project that has diverged; report drift instead.

| Repo type | What it is | Reference |
| - | - | - |
| System code repo | A deployed system with architecture, deployment, and modules | `reference/blueprint/system.md` |
| Library code repo | Code published for other repos to depend on; the public API surface is the primary artifact | `reference/blueprint/library.md` |
| Noncode repo | Docs are the repo's primary output — proposals, decisions, research; no code to describe | `reference/blueprint/noncode.md` |

Not every project needs every file in its reference tree; add what applies.

## Templates: content criteria by doc type

### Design docs

| Doc type | What it covers | Reference |
| - | - | - |
| System design doc | Module decomposition, public interface, data and control flow, data model and state, key algorithms, decisions, system e2e tests | `reference/templates/system_design.md` |
| Frontend design doc | Component tree, global state, data flow — for designs describing a frontend | `reference/templates/frontend_design.md` |
| UI design doc | Page structure and navigation, per-page interaction, chart visualization encoding | `reference/templates/ui_design.md` |

### Project docs

| Section | What it covers | Reference |
| - | - | - |
| Mission | Goal, problem statement, scope, stakeholders | `reference/templates/mission.md` |
| Usage scenarios | Concrete situations the project must handle | `reference/templates/usage_scenarios.md` |
| Roadmap | Vision, milestones, decisions log | `reference/templates/roadmap.md` |
| Tech stack | Languages, frameworks, toolchain, rationale | `reference/templates/tech_stack.md` |
| Concepts | Active domain vocabulary | `reference/templates/concepts.md` |
| API design (libraries) | Public API signatures, usage examples, design decisions | `reference/templates/api_design.md` |

### Other docs

| Doc type | What it covers | Reference |
| - | - | - |
| Research docs | Domain background, related-work comparison, design-option brainstorm | `reference/templates/research.md` |
| Dataset doc | Data acquisition, processing and description | `reference/templates/dataset.md` |

## Styles: markdown formatting

Opinionated doc formatting defaults — summary paragraph, numbered headings, no `---` separators, `-` bullets, key: value lists over two-column tables, Mermaid diagrams, `.markdownlint.json` — applied unless the project records different choices. Reference: `reference/styles/doc_style.md`.
