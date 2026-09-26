---
name: aqr-code-pattern
description: Universal code quality and design principles plus opinionated stack patterns and styles. Use when writing or reviewing source code or design docs; when implementing, migrating, or auditing UI against a design system; when writing or reviewing frontend code; or when applying this repo's code, test, or notebook style defaults.
disable-model-invocation: false
---

# aqr-code-pattern

Universal code quality and design principles — a floor, not a ceiling, applying regardless of language or stack. Apply at both design time and coding time. The references below layer stack-specific taste on top: architecture patterns and style defaults, opinionated rather than universal. Nothing here is copied into the project.

## Reference index

### Patterns

| Area | What it covers | Reference |
| - | - | - |
| Frontend architecture (React) | Global store vs component state, transforms at the fetch boundary, container/presentational split, component internal ordering, controller extraction | `reference/frontend_patterns.md` |
| Design system | Applying an existing design system to a project: build, extend, restyle, or audit UI | `reference/design_system.md` |

### Styles

Opinionated stack defaults — apply unless the project records a different choice.

| Area | What it covers | Reference |
| - | - | - |
| Code and tests (Python) | `TypedDict` over `dataclass`, `os.path` over `pathlib`, relative imports, naming, plain test functions, exact-equality assertions, ruff / pyright / pytest toolchain, `.python-version` / `pyproject.toml` config, editorconfig | `reference/styles/code_python.md` |
| Code and tests (JavaScript) | `.nvmrc` Node version pin, editorconfig, naming | `reference/styles/code_javascript.md` |
| Notebooks (Jupyter) | kernel pin, required header cells, root-finding boilerplate | `reference/styles/notebooks.md` |

## 1. Decomposition and responsibility

- Keep business logic independent of frameworks, UI, and data access: do not mix business logic into UI components or put database queries in controllers. Separate domain from infrastructure.
- One clear responsibility per unit; if its name needs "and", it is a candidate to split.
- A unit owns its state and hides its internals; callers depend on the contract, not the implementation.
- State lives with the unit that mutates it; lift shared state to the nearest common ancestor.
- Separate what a unit *is* (pure transformation) from what it *does* (owns state and side effects); mixing both is a smell.
- Do not abstract for fewer than three call sites; duplication is cheaper than a premature abstraction.

## 2. Library-first

Prefer an existing library over hand-rolled code for cross-cutting concerns (retry, validation, state management, auth). Custom code is justified only for domain-specific logic, performance-critical paths, or security-sensitive control. Record the chosen library and rationale in the tech-stack doc; the manifest holds the version pin, not the reason.

## 3. Public surface

- The public surface is a contract: cheap to propose, expensive to retract — default to exposing less.
- Type-annotate public APIs; untyped internal helpers are fine. Keep internals out of the public surface.
- If a caller needs to know an internal mechanism, that is a design problem, not a documentation problem.

## 4. Naming

- Named constants, not magic numbers or strings. Names describe what a thing is, not how it is built.
- Avoid dumping-ground names (`utils`, `helpers`, `common`, `shared`); name by domain responsibility so a file's purpose is clear from its name.
- Keep names consistent across the design-to-code boundary unless there is a recorded reason not to.

## 5. Error handling

- Handle errors at the boundary where something meaningful can be done. Do not swallow silently or re-raise without adding information.
- Validate at system boundaries; trust internal code and framework guarantees — do not defend against states that cannot occur.
- Errors are part of the contract: a function states how it fails, and surprising failure modes are bugs. Prefer loud, early failures over silent defaults.

## 6. Comments

- Comments explain why, not what. Omit when the why is obvious; one line when it is not.
- Do not reference issue numbers or PR titles in comments — they rot; they belong in commit messages.

## 7. Secrets and configuration

- Secrets are never hardcoded; read them from environment variables or out-of-tree config.
- Separate per-environment config (read at runtime) from true constants (defined in code). Mixing them makes deployments fragile.

## 8. File and module hygiene

- Do not create empty files (`__init__.py` excepted).
- Past ~300 lines, reconsider a module's scope before adding more.
- Co-locate what changes together; prefer early returns and avoid nesting beyond ~3 levels.

## 9. Testing

- Cover every pure function, and at least one realistic (non-happy-path) test for each public function.
- Test behavior, not implementation; mock slow, nondeterministic, or stateful dependencies and use real implementations for pure logic.
- Boundary tests for system boundaries: invalid input, missing fields, empty collections, off-by-one, large inputs.
- Avoid conceptually duplicated tests — combine minor input variations; split only when the logic path differs. Keep existing tests intact when modifying.

## 10. Before committing

- Run linters, formatters, and the type checker on touched files only — do not reformat the whole tree in an unrelated change.
- A new public function is covered by at least one realistic test.
- If the change affects a public surface or documented contract, update the matching design doc in the same change.
