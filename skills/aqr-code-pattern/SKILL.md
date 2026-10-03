---
name: aqr-code-pattern
description: Universal code quality and design principles plus opinionated stack patterns and styles. Use when writing or reviewing source code or design docs; when writing or reviewing frontend code; or when applying this repo's code, test, or notebook style defaults.
disable-model-invocation: false
---

# aqr-code-pattern

Universal code quality and design principles — a floor, not a ceiling, applying regardless of language or stack. Apply at both design time and coding time. The sections below reference stack-specific taste layered on top: architecture patterns and style defaults, opinionated rather than universal. Nothing here is copied into the project.

## 1. General criteria

### Design

- Keep business logic independent of frameworks, UI, and data access.
- Prefer a well-maintained library over hand-rolled code whenever one fits.
- Do not abstract for fewer than three call sites; duplication is cheaper than a premature abstraction.
- Past ~300 lines, a module is likely doing more than one thing: split it by responsibility instead of continuing to append.

### Configuration

- Keep secrets out of the code and the repository — read them from environment variables or out-of-tree config.

### Testing

- Cover each user-facing flow with at least one end-to-end test through the real stack; mock only external services that are slow or costly.
- Avoid conceptually duplicated tests — combine minor input variations into one test instead of adding near-copies.
- Keep existing tests intact when modifying — a test function might cover multiple cases; update only the cases the change affects and keep the rest.
- Run linters, formatters, and the type checker on touched files only. Do not reformat the whole tree in an unrelated change.

### Documentation

- If a change affects a public surface or involves a nontrivial decision or technique, update the doc to record that.

## 2. Frontend patterns

Opinionated architecture patterns for frontend code — apply unless the project records a different choice.

| Area | What it covers | Reference |
| - | - | - |
| Frontend architecture | Global store vs component state, transforms at the fetch boundary, container/presentational split, component internal ordering, controller extraction | `reference/frontend_patterns.md` |

## 3. Styles

Opinionated stack defaults — apply unless the project records a different choice.

| Area | What it covers | Reference |
| - | - | - |
| Code and tests (Python) | `TypedDict` over `dataclass`, `os.path` over `pathlib`, relative imports, naming, plain test functions, exact-equality assertions, ruff / pyright / pytest toolchain, `.python-version` / `pyproject.toml` config, editorconfig | `reference/styles/code_python.md` |
| Code and tests (JavaScript) | `.nvmrc` Node version pin, editorconfig, naming | `reference/styles/code_javascript.md` |
| Notebooks (Jupyter) | kernel pin, required header cells, root-finding boilerplate | `reference/styles/notebooks.md` |
