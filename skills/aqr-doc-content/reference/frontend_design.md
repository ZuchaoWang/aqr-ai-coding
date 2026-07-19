# Frontend design content criteria

Criteria for frontend design docs — when the design describes a frontend (a whole app or a single module).

## Purpose

A reviewer reads it and understands the component structure, state ownership, and interaction flow without reading component code.

## Content

- **Summary** — what this design covers and explicitly what it does not.
- **Component tree** — the component hierarchy; a reader can draw it from the text.
- **Per component** — for each component in the tree, record:
    - **State** — local state the component owns; what triggers a re-render.
    - **Data fetching** — what data the component loads, from where, and when (on mount, on interaction, periodically). Note loading and error states.
    - **Interaction** — which events the component handles, what state changes, and where the change lands (local, parent, or global store).
- **Global state store** — if the frontend uses a global store (e.g. Redux), describe the state shape: the slices, what each holds, and which components read or write each slice.
- **Other non-trivial design** — anything that deserves its own section: a new algorithm, cache design, optimistic-update strategy, etc.
- **Decisions** — decision records for significant design choices, one per entry.
