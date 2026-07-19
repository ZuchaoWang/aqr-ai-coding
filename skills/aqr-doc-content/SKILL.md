---
name: aqr-doc-content
description: Criteria for writing common documentation types (design, project, research, dataset). Use when writing or reviewing any such doc.
disable-model-invocation: false
---

# aqr-doc-content

Defines **what good content looks like** for common documentation types. When writing or reviewing a doc, load the reference file that matches the doc type and the repo type. Design and project docs are split by repo type because the shape of a good doc differs across them; research and dataset docs are shared because they already read generically.

## Reference files to look at

### Design docs

| Repo type | What the design doc covers | Reference |
| - | - | - |
| System | Module decomposition, public interface and API contract, data and control flow, data model and state, key algorithm, decisions, testing approach, with a UI/frontend overlay | `reference/design-system.md` |
| Library | Public API surface with stability tags and semver implications, usage examples and integration patterns, versioning and deprecation policy, internal decomposition as secondary | `reference/design-library.md` |
| Noncode | Problem statement, goals and non-goals, proposed design without reference to code in this repo, alternatives compared, decisions, open questions, forward reference to where it will be implemented | `reference/design-noncode.md` |

### Project docs

| Repo type | What the project docs cover | Reference |
| - | - | - |
| System | Mission and scope, usage scenarios, roadmap, tech-stack rationale, active domain concepts, user interface design | `reference/project-system.md` |
| Library | Mission framed as "what the library does and what it replaces", roadmap, API vocabulary, API reference with usage examples and supported-consumer matrix | `reference/project-library.md` |
| Noncode | Mission framed as "what knowledge or decisions this repo captures" with scope and audience folded in, roadmap of open decisions and pending docs | `reference/project-noncode.md` |

### Research and dataset docs (shared)

| Doc type | What it covers | Reference |
| - | - | - |
| Research docs | Domain background, related-work comparison, design-option brainstorm | `reference/research.md` |
| Dataset doc | Data acquisition, processing and description | `reference/dataset.md` |
