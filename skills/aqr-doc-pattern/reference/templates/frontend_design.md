# Frontend design content criteria

Criteria for frontend design docs — when the design describes a frontend (a whole app or a single module).

## 1. Purpose

A reviewer reads it and understands the component architecture, state ownership, and data flow without reading component code.

## 2. Content

- **Summary** — what this design covers and explicitly what it does not.
- **Component tree** — the component hierarchy down to component level; a one-line role for each component; a reader can draw it from the text.
- **Global state store** — if the frontend uses a global store (e.g. Redux), describe the slices, what each holds, and who owns and who reads each slice.
- **Data flow and fetching architecture** — how data enters the frontend, where transforms happen (the fetch boundary), and how data reaches components. State how loading and error states surface, at the architecture level.
- **Other non-trivial design** — anything that deserves its own section: a new algorithm, cache design, optimistic-update strategy, etc.
- **Decisions** — decision records for significant design choices, one per entry.

## 3. Constraints

- The component is the leaf: no per-component state, data-fetching, or event-handler breakdowns.
- No hook, class, or method names — those live in code.
- Page- and flow-level behavior belongs in the UI design doc; unit-test design belongs in code.
