# UI design content criteria

Criteria for the UI conceptual design doc — `ui_design.md`. Apply to frontend projects (web app, dashboard, visualization). This is the product-level UI requirement: what the UI must do for the user — pages, flows, behaviors. The visual style lives separately in `styles/`. The component build itself is a normal frontend module covered by a system design doc.

## 1. Pages

Purpose: a reviewer reads it and understands the UI's structure and navigation without reading component code.

Content:

1. **Pages and panels** — the pages (screens / views) and panels the UI is built from; each one's role, not its layout code.
2. **URL scheme** — routes, query params, redirects, defaults.
3. **Navigation** — how the user moves between pages; shared panels or chrome.

Constraints: state each page or panel's role; do not describe layout implementation.

## 2. Interactions

Purpose: a reviewer reads it and understands, per page, what the user can do and what happens.

Content — per page or panel:

1. **What it shows** — the data and state rendered.
2. **User inputs and their effect** — what the user can do and what changes.
3. **State changes** — how state transitions in response.
4. **Input-to-result flow** — e.g. an input cascade where one input gates the next.

Constraints: describe observable behavior, not implementation.

## 3. Visualization encoding

Purpose: a reviewer reads it and understands, per chart, how data is encoded visually and why. Omit this section if the project has no data visualization.

Content — for each chart:

1. **Field types and chart choice** — the data fields and the chart chosen.
2. **Field-to-channel mapping** — x, y, color, size, …, and why each mapping.
3. **Axes, scales, legends, labels** — the supporting visual structure.
4. **Interaction** — hover, filter, zoom, drill, etc.
5. **Performance** — behavior for large datasets.
