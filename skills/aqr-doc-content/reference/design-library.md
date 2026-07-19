# Library design content criteria

Criteria for design docs in a library code repo. The library is published for other repos to depend on, so the public API surface is the primary artifact. §1 lists the contents; §2 gives the design criteria.

## 1. Contents

A library design doc covers the public contract consumers depend on, plus the decisions behind it. If the library has internal modules, include decomposition at the level the library actually exposes — but the public surface is central; internal decomposition is secondary.

- **Summary** — what this design covers and explicitly what it does not.
- **Public API surface** — the contract this library exposes to its consumers. For each public symbol (function, type, trait, class, configuration surface):
    - **Signature and behavior contract** — inputs, outputs, side effects, error cases. The conceptual behavior, not a re-print of code.
    - **Stability tag** — `stable`, `experimental`, or `deprecated`, with the version where the tag last changed.
    - **Semver implications** — what kinds of changes to this symbol would require a major, minor, or patch release. State this even for `experimental` symbols (typically: "any change, no notice").
- **Usage examples and integration patterns** — the canonical ways consumers use this surface, as runnable examples or recipes. Include the integration shape (initialization, lifecycle, teardown) when it is non-trivial.
- **Versioning, backwards compatibility, and deprecation policy** — what the library guarantees across versions; how breaking changes are introduced (deprecation cycle, support windows, migration aids). If a published policy lives elsewhere (README, CHANGELOG), reference it; do not duplicate.
- **Decomposition (when the library has internal modules)** — name each internal module, its role, and the interface it must satisfy. Internal module decomposition is *not* the public surface; it is documented so future maintainers can evolve the implementation without breaking the contract.
- **Data model, state, and persistence** — conceptual structures the library owns (caches, connection pools, in-memory state). Omit if stateless.
- **Key algorithms** — for each non-trivial part: a well-known algorithm or pattern → just name it; otherwise brief pseudocode; trivial → nothing. A choice among several strategies is a decision (§2.3), not inline prose.
- **Key design and implementation decisions** — decision records, one per entry; format in §2.3.
- **Testing approach** — behaviors and edge cases to cover; what to mock versus use real. For a library, also cover how the public surface is tested for stability: the consumer-relevant matrix (OS, language runtime, supported dependency versions) and any compatibility tests against prior published versions.

## 2. Design criteria

### 2.1 Universal design criteria

- Reviewable without reading code; a reader can reproduce the design's shape from the text.
- Explicit boundaries — the design states what is inside and outside the public surface.
- Conceptual only; implementation detail lives in code.
- No code inventories — exhaustive file lists, internal class and function signatures, and test-function names belong in the code, not here. Document the designed contract — the public interface and why each part is exposed — not the derived internal inventory; review the code after execution.
- Be concise. Do not write code when a list of items will do — describe what and why, not how it is implemented.

Decomposition and responsibility (when the library has internal modules):

- Keep business logic independent of frameworks, UI, and data access: do not mix business logic into transport layers or put I/O into pure transformations. Separate domain from infrastructure.
- One clear responsibility per unit; if its name needs "and", it is a candidate to split.
- A unit owns its state and hides its internals; callers depend on the contract, not the implementation.
- Separate what a unit *is* (pure transformation) from what it *does* (owns state and side effects); mixing both is a smell.
- Do not abstract for fewer than three call sites; duplication is cheaper than a premature abstraction.

### 2.2 Recording design decisions

Record each significant decision as a short entry with three fields:

- **Context** — the forces that forced the choice.
- **Current decision** — what was chosen, in one line.
- **Alternatives** — options considered (including past decisions since reversed) and why they were not picked.

One decision per entry. When a decision is reversed, update Current decision and move the old one into Alternatives with a one-line reason.
