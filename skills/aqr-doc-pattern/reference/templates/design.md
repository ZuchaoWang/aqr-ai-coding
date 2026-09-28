# Design doc content criteria

Criteria for design docs at any level — the top-level design, a layer, or a module. Architecture and implementation shape are documented together; class- and function-level detail lives in code.

## 1. Purpose

A reviewer reads it and can reproduce the design's shape from the text, without reading code. Depth stops at child level: each child is a black box; what is inside a child lives in code and in the child's own design doc.

## 2. Content

- **Summary** — what this design covers and explicitly what it does not.
- **Public interface** — the contract this unit exposes upward, shown as implemented. Shape it by unit type:
  - Website / frontend — the URL route map: each route and what it shows.
  - Network service — the endpoints: method, path, request/response fields, error shape.
  - Other module — its conceptual contract: what it receives, returns, and guarantees. No signature lists.
- **Basic design** — the shape of the implementation, at conceptual level:
  - **Decomposition** — if the unit splits into children, name each child, its role and boundaries, and how the children compose and communicate. Component is the deepest documented level.
  - **Data model and state** — the conceptual structures this unit owns; who owns the state and how it persists. Omit if stateless.
  - **Data flow and control flow** — how data enters, is transformed, is stored, and leaves; request lifecycle, concurrency, failure and retry paths. Diagram when the flow is not obvious from prose, and describe it in prose first.
- **Nontrivial implementation hints** — only what an implementer could not infer from the interface and the basic design:
  - Key algorithms — a well-known algorithm or pattern → name it; otherwise brief pseudocode; trivial → nothing.
  - Key decisions — decision records, one per entry: what was chosen, alternatives considered, why.
  - Gotchas — concurrency, caching, or failure behavior that would surprise an implementer.

## 3. Constraints

- Conceptual only; implementation detail lives in code.
- No code inventories — no file lists; no class, method, or function names, whether of internals or of children; a child's contract is stated conceptually, not as a signature list.
- Be concise. Describe what and why, not how it is implemented.
