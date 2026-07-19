---
name: aqr-code-criteria
description: Universal code quality and design principles that apply regardless of language, framework, or project style. Use when writing or reviewing source code or design docs.
disable-model-invocation: false
---

# aqr-code-criteria

Universal code quality and design principles — a floor, not a ceiling, applying regardless of language or stack. These are reminders of what good code and good design look like; per-stack style (formatting, linting, framework mechanics) is not covered here. Apply at both design time (when writing design docs) and coding time (when writing or reviewing code).

## 1. Library-first

Prefer an existing library over hand-rolled code for cross-cutting concerns (retry, validation, state management, auth). Custom code is justified only for domain-specific logic, performance-critical paths, or security-sensitive control. Record the chosen library and rationale in the tech-stack doc; the manifest holds the version pin, not the reason.

## 2. Public surface

- The public surface is a contract: cheap to propose, expensive to retract — default to exposing less.
- Type-annotate public APIs; untyped internal helpers are fine. Keep internals out of the public surface.
- If a caller needs to know an internal mechanism, that is a design problem, not a documentation problem.

## 3. Naming

- Named constants, not magic numbers or strings. Names describe what a thing is, not how it is built.
- Avoid dumping-ground names (`utils`, `helpers`, `common`, `shared`); name by domain responsibility so a file's purpose is clear from its name.
- Keep names consistent across the design-to-code boundary unless there is a recorded reason not to.

## 4. Comments

- Comments explain why, not what. Omit when the why is obvious; one line when it is not.
- Do not reference issue numbers or PR titles in comments — they rot; they belong in commit messages.

## 5. Secrets and configuration

- Secrets are never hardcoded; read them from environment variables or out-of-tree config.
- Separate per-environment config (read at runtime) from true constants (defined in code). Mixing them makes deployments fragile.

## 6. Error handling

- Handle errors at the boundary where something meaningful can be done. Do not swallow silently or re-raise without adding information.
- Validate at system boundaries; trust internal code and framework guarantees — do not defend against states that cannot occur.
- Errors are part of the contract: a function states how it fails, and surprising failure modes are bugs. Prefer loud, early failures over silent defaults.

## 7. File and module hygiene

- Do not create empty files (`__init__.py` excepted).
- Past ~300 lines, reconsider a module's scope before adding more.
- Co-locate what changes together; prefer early returns and avoid nesting beyond ~3 levels.

## 8. Testing

- Cover every pure function, and at least one realistic (non-happy-path) test for each public function.
- Test behavior, not implementation; mock slow, nondeterministic, or stateful dependencies and use real implementations for pure logic.
- Boundary tests for system boundaries: invalid input, missing fields, empty collections, off-by-one, large inputs.
- Avoid conceptually duplicated tests — combine minor input variations; split only when the logic path differs. Keep existing tests intact when modifying.

## 9. Before committing

- Run linters, formatters, and the type checker on touched files only — do not reformat the whole tree in an unrelated change.
- A new public function is covered by at least one realistic test.
- If the change affects a public surface or documented contract, update the matching design doc in the same change.

## 10. Decomposition and responsibility

- Keep business logic independent of frameworks, UI, and data access: do not mix business logic into UI components or put database queries in controllers. Separate domain from infrastructure.
- One clear responsibility per unit; if its name needs "and", it is a candidate to split.
- A unit owns its state and hides its internals; callers depend on the contract, not the implementation.
- State lives with the unit that mutates it; lift shared state to the nearest common ancestor.
- Separate what a unit *is* (pure transformation) from what it *does* (owns state and side effects); mixing both is a smell.
- Do not abstract for fewer than three call sites; duplication is cheaper than a premature abstraction.

## 11. Frontend component design

- Presentational vs. container: presentational components are stateless (props in, UI out, reusable); containers are stateful (own data and logic, pass props down). Mixing both is a smell.
- Locally-shared state lives at the nearest common ancestor of its consumers; drilling it through layers that don't use it signals it is owned too high.
- Broadly-shared or persistent state (auth, theme, session, cached data) lives in a store or context and is read directly by the components that need it — never drilled down through intermediaries as props.
- Extract a component when the same UI is needed a second time (to avoid copy-paste); also split a component that mixes the two roles — pull data fetching and state into a container, and keep the presentational UI pure.
- A component does not define its own absolute position; that is set by its parent's layout. The component only sizes and lays out its own children.
