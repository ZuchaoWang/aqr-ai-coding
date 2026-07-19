# Frontend design content criteria

Criteria for frontend design docs — when the design describes a frontend (a whole app or a single module). For a frontend, decomposition is the component tree, and the inter-component interface is carried by state and interactions — not prop wiring, which is an implementation detail.

## Purpose

A reviewer reads it and understands the component structure, state ownership, and interaction flow without reading component code.

## Content

- **Component tree** — the component hierarchy; a reader can draw it from the text.
- **State list** — which component owns each piece of state; local, shared, server/cache.
- **Interactions** — which component handles which event and where the state change lands.
