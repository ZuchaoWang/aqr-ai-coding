---
name: aqr-doc-content
description: Criteria for writing common documentation types (code design, project, UI design, UI style, research, dataset). Use when writing or reviewing any such doc.
disable-model-invocation: false
---

# aqr-doc-content

Defines **what good content looks like** for common documentation types. When writing or reviewing a doc, load the matching reference and apply the sections that fit; per-repo-type variants are noted inline as overlays.

## Reference files to look at

| Doc type | What it covers | Reference |
| - | - | - |
| Code design doc | Module decomposition, public interface and API contract, data and control flow, data model and state, key algorithm, decisions, testing approach, with UI/frontend and library overlays | `reference/code-design.md` |
| Project docs | Mission and scope, usage scenarios, roadmap, tech-stack rationale, active domain concepts, with library and noncode overlays | `reference/project.md` |
| UI design doc | Screen structure and navigation, per-screen interaction, chart visualization encoding | `reference/ui_design.md` |
| UI style doc | Palette, typography, shared component styling | `reference/style.md` |
| Research docs | Domain background, related-work comparison, design-option brainstorm | `reference/research.md` |
| Dataset doc | Data acquisition, processing and description | `reference/dataset.md` |
