# UI design content criteria

Criteria for the project-level UI design docs in `uidesign/` — apply to frontend projects (web app, dashboard, visualization). These are product decisions: what the UI should be, not how the components are built. The component build itself is a normal frontend module covered by a design doc in `implementation/`.

State anything shared across screens once, in the matching file.

## 1. Screens

Purpose: a reviewer reads it and understands the UI's structure and navigation without reading component code.

Content:

1. **Screens and panels** — the screens (pages / views) and panels the UI is built from; each one's role, not its layout code.
2. **URL scheme** — routes, query params, redirects, defaults.
3. **Navigation** — how the user moves between screens; shared panels or chrome.

Constraints: state each screen or panel's role; do not describe layout implementation.

## 2. Interactions

Purpose: a reviewer reads it and understands, per screen, what the user can do and what happens.

Content — per screen or panel:

1. **What it shows** — the data and state rendered.
2. **User inputs and their effect** — what the user can do and what changes.
3. **State changes** — how state transitions in response.
4. **Input-to-result flow** — e.g. an input cascade where one input gates the next.

Constraints: describe observable behavior, not implementation.

## 3. Visual language

Purpose: a reviewer reads it and understands the shared visual style the UI is built in.

Content — state once, when shared across screens:

1. **Target viewport** — fixed size or responsive breakpoints.
2. **Palette** — each color and its role.
3. **Typography** — typefaces, roles, key sizes.
4. **Shared component styling** — conventions shared across components.

Constraints: note a screen only where it overrides the shared style.

## 4. Visualization encoding

Purpose: a reviewer reads it and understands, per chart, how data is encoded visually and why. Omit this file if the project has no data visualization.

Content — for each chart:

1. **Field types and chart choice** — the data fields and the chart chosen.
2. **Field-to-channel mapping** — x, y, color, size, …, and why each mapping.
3. **Axes, scales, legends, labels** — the supporting visual structure.
4. **Interaction** — hover, filter, zoom, drill, etc.
5. **Performance** — behavior for large datasets.
