---
name: aqr-doc-content
description: Criteria for writing common documentation types (system design, project, UI design, UI style design, research, dataset). Use when writing or reviewing any such doc.
disable-model-invocation: false
---

# aqr-doc-content

Defines **what good content looks like** for common documentation types. When writing or reviewing a doc, load the matching reference and apply the sections that fit; per-repo-type variants are noted inline.

## Reference files to look at

### Design docs

| Doc type | What it covers | Reference |
| - | - | - |
| System design doc | Module decomposition, public interface and API contract, data and control flow, data model and state, key algorithm, decisions, testing approach, with a UI/frontend overlay | `reference/system_design.md` |
| UI design doc | Screen structure and navigation, per-screen interaction, chart visualization encoding | `reference/ui_design.md` |
| UI style design doc | Palette, typography, shared component styling, mockups and visual references | `reference/style_design.md` |

### Project docs

| Section | What it covers | Reference |
| - | - | - |
| Mission | Goal, problem statement, scope, stakeholders | `reference/project/mission.md` |
| Usage scenarios | Concrete situations the project must handle | `reference/project/usage_scenarios.md` |
| Roadmap | Vision, milestones, decisions log | `reference/project/roadmap.md` |
| Tech stack | Languages, frameworks, toolchain, rationale | `reference/project/tech_stack.md` |
| Concepts | Active domain vocabulary | `reference/project/concepts.md` |
| API reference (libraries) | Public surface reference, stability, usage examples | `reference/project/api.md` |
| API design (libraries) | Why the API is shaped this way, stability and versioning decisions | `reference/project/api_design.md` |

### Other docs

| Doc type | What it covers | Reference |
| - | - | - |
| Research docs | Domain background, related-work comparison, design-option brainstorm | `reference/research.md` |
| Dataset doc | Data acquisition, processing and description | `reference/dataset.md` |
