# System design content criteria

Criteria for system design docs. §1 lists the contents to include; §2 gives the doc-quality criteria and decision-recording format. For frontend design docs, see `frontend_design.md`. Design quality principles (decomposition, responsibility, state management) live in `aqr-code-criteria` — they apply at both design time and coding time.

## 1. Universal contents (every design doc)

Every design doc — system, layer, or module — covers the same categories; only the depth scales by level. A unit that has children describes those children as black boxes (their interface and how they compose); a leaf module carries the implementation detail. If the unit follows a well-known pattern (B/S, C/S, MVC, MVVM, microservices, event-driven, …), name it and state only where this unit deviates — do not re-explain the pattern, which already implies its components, data flow, and control flow.

- **Summary** — what this design covers and explicitly what it does not.
- **Public interface (this unit's own) and its conceptual implementation** — the upward contract this unit's parent defined for it, restated and shown as implemented. Shape it by unit type:
  - Website / frontend — the URL route map: each route and what it shows. For frontend component decomposition, see `frontend_design.md`.
  - Network service — the endpoints: method, path, request/response fields, error shape.
  - Other module — the signatures of its public API: the functions or methods it exposes.
- **Decomposition and children's interfaces** — if this unit splits into children, name each child module, its role and boundaries, the public interface it must satisfy (the downward, black-box contract this unit imposes on it), and how the children communicate and compose. For a website, the inter-module interface is the B/S API — the request/response contract between browser and server, and between frontend and backend modules.
- **Data model, state, and persistence** — the conceptual structures this unit owns; what state exists, who owns it, and how it persists. Conceptual level, not a full DDL. Omit if stateless.
- **Data flow and control flow** — how data enters, is transformed, is stored, and leaves; the request lifecycle, state transitions, concurrency, caching, failure and retry paths. Add a diagram when the flow is not obvious from prose, and describe it in prose first.
- **Key algorithms** — for each non-trivial part: a well-known algorithm or pattern → just name it; otherwise brief pseudocode; trivial → nothing. A choice among several strategies is a decision (§3), not inline prose.
- **Key design and implementation decisions** — decision records, one per entry; format in §3.
- **Testing approach** — behaviors and edge cases to cover; what to mock versus use real.

## 2. Doc-quality criteria

- Reviewable without reading code; a reader can reproduce the design's shape from the text.
- Explicit boundaries — every unit states what is inside and outside its responsibility.
- Conceptual only; implementation detail lives in code.
- No code inventories — exhaustive file lists, internal class and function signatures, and test-function names belong in the code, not here. Document the designed contract — the public interface and why each part is exposed — not the derived internal inventory; review the code after execution.
- Be concise. Do not write code when a list of items will do — describe what and why, not how it is implemented.

## 3. Recording design decisions

Record each significant decision as a short entry with three fields:

- **Context** — the forces that forced the choice.
- **Current decision** — what was chosen, in one line.
- **Alternatives** — options considered (including past decisions since reversed) and why they were not picked.

One decision per entry. When a decision is reversed, update Current decision and move the old one into Alternatives with a one-line reason.
