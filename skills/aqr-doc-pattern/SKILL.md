---
name: aqr-doc-pattern
description: The recommended docs layout plus content criteria for common documentation types and markdown formatting defaults. Use when laying out a project's docs or deciding where a doc belongs; when writing or reviewing any project doc (implementation design, UI design, project, research, dataset); or when applying this repo's markdown style defaults.
disable-model-invocation: false
---

# aqr-doc-pattern

Three layers: the blueprint says what docs a project should have and where (layout only, not content); the templates define what good content looks like for each doc type; the style section below sets markdown formatting defaults. When writing or reviewing a doc, load the matching template and apply the sections that fit.

## 1. Blueprint: doc layout

Every project — code or not — starts from this root layout:

```
README.md                # what the project is and how to start 
CLAUDE.md                # agent instructions and toolchain specifics; also a brief top-level directory map
docs/                    # the detailed documentation, including background, research, design, implementation, etc.
```

For code project (systems and libraries), the recommended file structure under `docs/` folder can be found in `reference/blueprint.md`. Use it to bootstrap a new docs tree or to audit an existing one for drift. The blueprint is a baseline, not a prescription — do not impose it on a project that has diverged; report drift instead. We do not cover noncode projects because docs are their primary output and the structure varies too much to prescribe.

## 2. Templates: doc content

Groups follow the blueprint's docs tree (`project/`, `research/`, `implementation/`, `data/`; research and data share one group here); rows within a group follow the blueprint's file order.

### 2.1 Project docs

| Doc type | What it covers | Reference |
| - | - | - |
| Mission | Goal, problem statement, scope, stakeholders | `reference/templates/mission.md` |
| Status | Current stage, next tasks, decisions log | `reference/templates/status.md` |
| Usage scenarios | Concrete situations the project must handle | `reference/templates/usage_scenarios.md` |
| Concepts | Active domain vocabulary | `reference/templates/concepts.md` |
| UI design | Page hierarchy with URL scheme, per-page and per-UI goals and rationale, per-UI visualization encoding and user interaction | `reference/templates/ui_design.md` |
| API design | Project level public API — a library's symbols, a pure data API, or a website's server-side API (B/S): signatures or endpoints, usage examples, decisions | `reference/templates/api_design.md` |

### 2.2 Implementation docs

| Doc type | What it covers | Reference |
| - | - | - |
| Implementation design (system or module) | How a predefined interface is implemented: summary, data and state, decomposition, data and control flow, nontrivial implementation hints — frontend app variations included | `reference/templates/implementation_design.md` |
| Tech stack | Languages, frameworks, toolchain, rationale | `reference/templates/tech_stack.md` |
| Deploy | Overview (modes and variants), configuration, run instructions per mode | `reference/templates/deploy.md` |

### 2.3 Other docs

| Doc type | What it covers | Reference |
| - | - | - |
| Research docs | Domain background, related-work comparison, design-option brainstorm | `reference/templates/research.md` |
| Dataset doc | Data acquisition, processing and description | `reference/templates/dataset.md` |

## 3. Style: doc formatting

Opinionated formatting defaults, applied unless the project records different choices:

### 3.1 Structure

- Place a summary paragraph after the title and before subsections in technical docs.
- Use numbered headings (`## 1. Overview`, `### 1.1 Motivation`) so cross-references stay stable.
- Parallel sections adopt a similar structure, field organization, and description order, so they compare side by side and stay easy to implement and maintain.

### 3.2 Markdown conventions

- Do not use `---` horizontal separators; restructure instead. Use `-` for list items, not `*`.
- For two-column tables, convert to key: value lists instead.
- Render diagrams as Mermaid fenced blocks (` ```mermaid `), not ASCII art or image files, so they render inline and stay editable as text.
- Wrap code and identifiers in backticks (e.g. `foo-bar`).
- A bare URL with no caption uses the `<url>` form; a URL with caption text, and file paths, use the `[text](url)` form.

### 3.3 Chinese text

- Use full-width punctuation for Chinese sentences （，。：；、）.
- Leave a space between Chinese and English words or numbers (e.g. `RPKI 风险`, `2026 年`).

### 3.4 Definitions

- Introduce concepts with positive definitions — responsibility, content, value — not long "what it is not" passages.
- Define each term, metric, algorithm, UI element, or design concept at its first mention; later text reuses the meaning, and for a close variant says "similar to X" and states only the difference — no re-explaining common knowledge.
