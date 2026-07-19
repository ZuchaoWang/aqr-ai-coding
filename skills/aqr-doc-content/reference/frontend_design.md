# Frontend design content criteria

Criteria for frontend design docs — when the design describes a frontend (a whole app or a single module).

## Purpose

A reviewer reads it and understands the component structure, state ownership, and interaction flow without reading component code.

## Content

- **Component tree** — the component hierarchy; a reader can draw it from the text.
- **State list** — which component owns each piece of state; local, shared, server/cache.
- **Interactions** — which component handles which event and where the state change lands.
