# UI style content criteria

Criteria for the visual style guide in `styles/`. Covers the visual language shared across the UI — the look and feel, not what the UI does. Separate from `ui_design.md` (conceptual design) because visual style changes at a different rate and is referenced across all features and screens. The folder can hold any files: markdown for the criteria below, plus images, HTML mockups, or links to design files as concrete references.

## 1. Palette

Purpose: a reviewer or implementer reads it and knows which colors to use and where.

Content — for each color:

- **Role** — what it is used for (primary action, background, error state, border, etc.).
- **Value** — the color value (hex, hsl, or design-token name).
- **Variants** — if applicable (hover, active, disabled).

Constraints: name colors by role, not by hue ("primary-action", not "blue-500"). If a token system or palette file exists elsewhere, reference it rather than duplicating.

## 2. Typography

Purpose: a reviewer or implementer reads it and knows which typefaces and sizes to use.

Content:

1. **Typefaces** — the font families in use and where each applies (body, heading, mono).
2. **Roles** — heading, body, caption, code, etc.
3. **Key sizes** — the type scale, with the role each size maps to.
4. **Line height and spacing** — if shared across roles.

Constraints: state the scale once; do not repeat per screen.

## 3. Components

Purpose: a reviewer or implementer reads it and knows how a shared component looks and behaves visually across the app.

Content — for each shared component (button, input, card, dialog, etc.):

1. **Visual conventions** — how it looks across the app.
2. **States** — default, hover, active, focused, disabled, error.
3. **Variants** — if applicable (primary, secondary, destructive, etc.).

Constraints: cover only components shared across screens. One-off components belong in their feature's design doc.

## 4. Mockups and visual references

Purpose: an implementer sees the style applied concretely, not just described in prose. Visual references make the style unambiguous and catch drift early.

Content:

1. **Pictures and screenshots** — annotated images showing the style applied to real screens or components.
2. **HTML mockups** — static HTML files demonstrating the style in a runnable form; useful for reviewing spacing, typography, and color in context.
3. **Design file links** — links to Figma, Sketch, or other design tool files, if the source of truth lives there.
4. **Annotations** — call out where the mockup deviates from the shared style, if anywhere.

Constraints: mockups illustrate the style defined in §1–§3; they do not replace it. If a mockup and the written criteria disagree, one of them is wrong — resolve which.
