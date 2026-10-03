---
name: aqr-presentation
description: Opinionated PowerPoint (.pptx) deck rules and style defaults — enforced content-per-slide rules and a render-and-inspect done gate, plus overridable slide, text, and bullet defaults. Use when creating or editing a deck in a project that adopts these rules.
disable-model-invocation: false
---

# aqr-presentation

Opinionated `.pptx` deck rules and defaults. Use the `pptx` skill for all pptx work; the rules below override or extend that skill's defaults. Nothing here is copied into the project.

## 1. Enforced rules

Always apply — these hold even when the project records different style choices.

### 1.1 Content per slide

- Each distinct group of examples in the source material is its own slide — do not merge multiple example groups onto one slide.
- Code slides may include a short plain-text note below the code block in normal body font.
- Mermaid diagrams are rendered to images first (e.g. via `mmdc`), then inserted as a picture — do not embed mermaid source text in slides.

### 1.2 Before declaring done

- After generating a `.pptx`, render all slides to images and visually inspect every slide. Do not declare done until you have checked for clipping, overlapping elements, misalignment, and unreadable text.

## 2. Style defaults

Apply unless the project or user explicitly specifies otherwise.

### 2.1 Slide setup

- Slide dimensions: 16:10 aspect ratio (25.4 cm × 15.875 cm, i.e. `widescreen` in python-pptx).
- Plain white background; no decorative graphics. Content only: text, bullet points, tables, and simple charts.
- Slide layout: title slide for the first slide, then content slides with a slide title and body area.

### 2.2 Text and bullets

- Title font size is dynamic: shorter titles use a larger font, longer titles a smaller font. Never let a title wrap to a second line.
- Subtitle text on the title slide is visually smaller than the main title.
- Body text is clearly readable; level-1 bullets slightly smaller than level-0. Body must not start with a blank line.
- Bullet symbols by level: level 0 → `•`, level 1 → `▸`, level 2 → `◦`.
- Bullet left margin (marL) by level: level 0 → 304800 EMU (0.33"), level 1 → 762000 EMU (0.83"), level 2 → 1219200 EMU (1.33"). Bullet hangs 228600 EMU (0.25") left of the text start (indent = −228600).
- Body text box leaves at least 0.6" on the right. Bullet text must not wrap beyond two lines — shorten or split if needed.
