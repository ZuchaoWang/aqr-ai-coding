# Implementation design content criteria

Criteria for implementation design docs — the top-level doc (`implementation/design.md`) and one design doc per module (`implementation/modules/`). The doc does not define the public interface: at system level the interface is defined in `project/api_design.md`, at submodule level in the parent module's implementation design doc — this doc starts from the predefined interface and describes how it is implemented. Section 2 defines the general content; section 3 defines the variations for a frontend app. Architecture and implementation shape are documented together; class- and function-level detail lives in code.

## 1. Purpose

A reviewer reads it and can reproduce the design's shape from the text, without reading code. Depth stops at child level: each child is a black box; what is inside a child lives in code and in the child's own implementation design doc.

## 2. Content

- **Summary** — what this design covers and explicitly what it does not.
- **Data and state** — the conceptual structures this unit owns; who owns the state and how it persists. Include when the unit persists or shares state.
- **Decomposition** — only for a unit with child modules. For each child module: its name, role, and boundaries, and the interface the parent uses — what it receives, returns, and guarantees, as a conceptual contract (no signature lists). State how the children compose and communicate; the child module is the deepest documented level.
- **Data and control flow** — how data flows through the unit and how control proceeds: the request path, concurrency, failure and retry paths. Include when the flow is complex; describe it in prose first, and diagram when prose alone is not clear.
- **Nontrivial implementation hints** — only what an implementer could not infer from the predefined interface and the content above:
  - Key algorithms — a well-known algorithm or pattern → name it; otherwise brief pseudocode; trivial → nothing.
  - Key decisions — decision records, one per entry: what was chosen, alternatives considered, why.
  - Gotchas — concurrency, caching, or failure behavior that would surprise an implementer.

## 3. Frontend app variations

For a frontend app or frontend module, the same sections take this shape:

- **Interface** — the routes are the interface; they are defined in the UI design doc, not here.
- **Data and state** — the global store: the slices, what each holds, and who owns and who reads each slice.
- **Decomposition** — the component tree: the hierarchy down to component level, a one-line role for each component. The component is the leaf: no per-component state, data-fetching, or event-handler breakdowns.
- **Data and control flow** — the fetching architecture: the fetch boundary — where transforms happen and how data reaches components; how loading and error states surface.
- **Nontrivial implementation hints** — same role as in section 2; frontend examples: cache design, optimistic-update strategy.
- Page- and flow-level behavior belongs in the UI design doc.

## 4. Constraints

- Conceptual only; implementation detail — including unit-test design — lives in code.
- No code inventories — no file lists; no hook, class, method, or function names, whether of internals or of children; a child's contract is stated conceptually, not as a signature list.
- Be concise. Describe what and why, not how it is implemented.
