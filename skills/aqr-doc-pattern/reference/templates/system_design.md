# System design content criteria

Criteria for system design docs — design docs for system, layer, or module levels.

## 1. Purpose

A reviewer reads it and can reproduce the design's shape from the text, without reading code.

## 2. Content

Depth stops at component level. Each child is a black box; what is inside a component lives in code. If a unit follows a well-known pattern, name it and state only where this unit deviates.

- **Summary** — what this design covers and explicitly what it does not.
- **Public interface** — the upward contract this unit's parent defined for it, restated and shown as implemented. Shape it by unit type:
  - Website / frontend — the URL route map: each route and what it shows.
  - Network service — the endpoints: method, path, request/response fields, error shape.
  - Other module — its conceptual contract: what it receives, returns, and guarantees. No signature lists.
- **Decomposition and children** — if this unit splits into children, name each child module, its role and boundaries, its conceptual contract (what it receives, returns, guarantees), and how the children communicate and compose. Component is the deepest documented level.
- **Data model, state, and persistence** — the conceptual structures this unit owns; what state exists, who owns it, and how it persists. Conceptual level, not a full DDL. Omit if stateless.
- **Data flow and control flow** — how data enters, is transformed, is stored, and leaves; the request lifecycle, state transitions, concurrency, caching, failure and retry paths. Add a diagram when the flow is not obvious from prose, and describe it in prose first.
- **Key algorithms** — for each non-trivial part: a well-known algorithm or pattern → just name it; otherwise brief pseudocode; trivial → nothing. A choice among several strategies is a decision, not inline prose.
- **Key design and implementation decisions** — decision records, one per entry.
- **System e2e tests** — the end-to-end flows and scenarios to test for this system, and for each, conceptually how to test it. Unit-test design, test names, and method names live in code.

## 3. Constraints

- Explicit boundaries — every unit states what is inside and outside its responsibility.
- Conceptual only; implementation detail lives in code.
- No code inventories — no file lists; no class, method, or function names, whether of internals or of children; no test names. A child's contract is stated conceptually, not as a signature list.
- Be concise. Do not write code when a list of items will do — describe what and why, not how it is implemented.
