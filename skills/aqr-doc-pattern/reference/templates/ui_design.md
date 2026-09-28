# UI design content criteria

Criteria for the UI conceptual design doc. Apply to frontend projects (web app, dashboard, visualization).

## 1. Purpose

A reviewer reads it and understands the UI's structure, navigation, and per-page behavior without reading component code.

## 2. Content

1. **Page hierarchy** — the pages (screens / views) organized as a hierarchy; each page's URL scheme (routes, query params, defaults, redirects); how the user navigates between pages, including shared chrome.
2. **Goals and rationale** — for each page, and for each UI within a page: what it is for and why it is shaped this way.
3. **Visualization and user interaction** — for each UI: for data-driven UIs, the visualization encoding — field types, chart choice, field-to-channel mapping and why; then the user interaction — the user inputs and their effect, how state changes, and the input-to-result flow (e.g. an input cascade where one input gates the next). Omit encoding where the project has no visualization.

## 3. Constraints

- Describe goal and rationale before observable behavior, do not describe implementation.
