---
name: aqr-code-pattern
description: Universal code quality and design principles plus opinionated stack patterns and styles. Use when writing or reviewing source code or design docs; when writing or reviewing frontend code; or when applying this repo's code, test, or notebook style defaults.
disable-model-invocation: false
---

# aqr-code-pattern

Universal code quality and design principles — a floor, not a ceiling, applying regardless of language or stack. Apply at both design time and coding time. The references below layer stack-specific taste on top: architecture patterns and style defaults, opinionated rather than universal. Nothing here is copied into the project.

## 1. Reference index

### Patterns

| Area | What it covers | Reference |
| - | - | - |
| Frontend architecture (React) | Global store vs component state, transforms at the fetch boundary, container/presentational split, component internal ordering, controller extraction | `reference/frontend_patterns.md` |

### Styles

Opinionated stack defaults — apply unless the project records a different choice.

| Area | What it covers | Reference |
| - | - | - |
| Code and tests (Python) | `TypedDict` over `dataclass`, `os.path` over `pathlib`, relative imports, naming, plain test functions, exact-equality assertions, ruff / pyright / pytest toolchain, `.python-version` / `pyproject.toml` config, editorconfig | `reference/styles/code_python.md` |
| Code and tests (JavaScript) | `.nvmrc` Node version pin, editorconfig, naming | `reference/styles/code_javascript.md` |
| Notebooks (Jupyter) | kernel pin, required header cells, root-finding boilerplate | `reference/styles/notebooks.md` |

## 2. General criteria

### Design

- Keep business logic independent of frameworks, UI, and data access. Separate pure transformation from state and side effects.
- Prefer an existing library for cross-cutting concerns (retry, validation, auth). Record the choice and rationale in the doc.
- Do not abstract for fewer than three call sites; duplication is cheaper than a premature abstraction.
- Past ~300 lines, reconsider a module's scope. Co-locate what changes together.

### Configuration

- Separate per-environment config (read at runtime) from true constants (defined in code). Secrets come from environment variables or out-of-tree config.

### Testing

- Avoid conceptually duplicated tests
- Keep existing tests intact when modifying.
- Run linters, formatters, and the type checker on touched files only. Do not reformat the whole tree in an unrelated change.

### Documentation

- If a change affects a public surface or involves a nontrivial decision or technique, update the doc to record that.
