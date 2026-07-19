# UI design content criteria

Criteria for the UI conceptual design doc. Apply to frontend projects (web app, dashboard, visualization).

## 1. Purpose

A reviewer reads it and understands the UI's structure, navigation, and per-page behavior without reading component code.

## 2. Content

- **Pages and panels** — the pages (screens / views) and panels the UI is built from; each one's role, not its layout code.
- **URL scheme** — routes, query params, redirects, defaults.
- **Navigation** — how the user moves between pages; shared panels or chrome.
- **Interactions** (per page or panel) — what it shows, the user inputs and their effect, how state changes, and the input-to-result flow (e.g. an input cascade where one input gates the next).
- **Visualization encoding** (for charts) — field types and chart choice, field-to-channel mapping (x, y, color, size, …) and why, axes/scales/legends/labels, interaction (hover, filter, zoom, drill), and performance for large datasets. Omit if the project has no data visualization.

## 3. Constraints

- State each page or panel's role; do not describe layout implementation.
- Describe observable behavior, not implementation.
