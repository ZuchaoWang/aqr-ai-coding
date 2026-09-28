# Implementation design content criteria

Criteria for implementation design docs — the top-level doc for the system and one design doc per module. The doc does not define the public interface: at system level the interface is defined in the api design doc, at submodule level in the parent module's implementation design doc — this doc starts from the predefined interface and describes how it is implemented. Section 2 defines the general content; section 3 defines the variations for a frontend app. Architecture and implementation shape are documented together; class- and function-level detail lives in code.

## 1. Purpose

A reviewer reads it and can reproduce the design's shape from the text, without reading code. Depth stops at child level: each child is a black box; what is inside a child lives in code and in the child's own implementation design doc.

## 2. Content

- **Summary** — what this design covers and explicitly what it does not.
- **Data and state** — each state structure this unit owns, in detail: its type, an explanation (what it is for), and how it persists. Include when the unit persists or shares state.
- **Decomposition** — only for a unit with child modules. For each child module: its name, role, and boundaries, and the interface the parent uses — what it receives, returns, and guarantees, as a conceptual contract (no signature lists). State the composition — which children use or contain which; the child module is the deepest documented level.
- **Data and control flow** — how data and control move through the unit at runtime, along the composition above: the request path — through which children, in what order; concurrency, failure and retry paths. Include when the flow is complex; describe it in prose first, and diagram when prose alone is not clear.
- **Nontrivial implementation hints** — only what an implementer could not infer from the predefined interface and the content above:
  - Key algorithms — a well-known algorithm or pattern → name it; otherwise brief pseudocode; trivial → nothing.
  - Key decisions — decision records, one per entry: what was chosen, alternatives considered, why.
  - Gotchas — concurrency, caching, or failure behavior that would surprise an implementer.

## 3. Frontend app variations

For a frontend app or frontend module, section 2 applies with two variations:

- **Data and state** — state which state is globally managed (the store: its slices, what each holds, and who reads each) and which is per component.
- **Decomposition** — states the component tree: the hierarchy down to component level, a one-line role for each component; the component is the leaf, with no per-component state, data-fetching, or event-handler breakdowns.

## 4. Constraints

- Trivial implementation detail — including unit-test design — lives in code.
- No code inventories — no file lists and no method or function names, whether of internals or of children; a child's contract is stated conceptually, not as a signature list.
- Be concise. Describe what and why, not how it is implemented.
