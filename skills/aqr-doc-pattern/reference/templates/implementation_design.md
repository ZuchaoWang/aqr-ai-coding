# Implementation design content criteria

Criteria for implementation design docs — the top-level doc (`implementation/design.md`) and one design doc per module (`implementation/modules/`). Architecture and implementation shape are documented together; class- and function-level detail lives in code. When the design describes a frontend, use the frontend implementation design criteria (`frontend_implementation_design.md`) instead.

## 1. Purpose

A reviewer reads it and can reproduce the design's shape from the text, without reading code. Depth stops at child level: each child is a black box; what is inside a child lives in code and in the child's own implementation design doc.

## 2. Content

- **Summary** — what this design covers and explicitly what it does not.
- **Public interface** — the contract this system/module exposes upward:
  - Network service — method, path, request/response fields, error shape.
  - Other module — function/class name, arguments, return values, and guarantees.
- **Basic design** — the shape of the implementation, at conceptual level. Include only the parts that apply:
  - **Decomposition** — only for a system/module with child modules: name each child, its role and boundaries, and how the children compose and communicate.
  - **Data model and state** — only for a system/module with complex state: state definition, owning component and persistency mechanism; how data enters, is transformed, is stored, and leaves.
- **Nontrivial implementation hints** — only what an implementer could not infer from the interface and the basic design:
  - Key algorithms — a well-known algorithm or pattern → name it; otherwise brief pseudocode; trivial → nothing.
  - Key decisions — decision records, one per entry: what was chosen, alternatives considered, why.
  - Gotchas — concurrency, caching, or failure behavior that would surprise an implementer.

## 3. Constraints

- Conceptual only; implementation detail lives in code.
- No code inventories — no file lists; no class, method, or function names, whether of internals or of children; a child's contract is stated conceptually, not as a signature list.
- Be concise. Describe what and why, not how it is implemented.
