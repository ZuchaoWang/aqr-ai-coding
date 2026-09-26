# Tech stack content criteria

## 1. Purpose

A reviewer reads it and understands what tools are in use and why those choices were made. Omit for a noncode repo.

## 2. Content

1. **Languages and runtimes** — one line per language: version, where the pin lives.
2. **Frameworks and libraries** — one line per key dependency: what it does here, why it was chosen over alternatives. When a library was chosen over custom code (library-first), say so and name the custom code it replaces — e.g. "cockatiel — retry logic, chosen over hand-rolled retry."
3. **Toolchain** — linters, formatters, type checkers, test runners, build tools.
4. **Rationale note** — any non-obvious choice that would confuse a reader without explanation.

## 3. Constraints

Target ~1 page. If a tool is standard and the reason is obvious ("pytest because it's the Python default"), omit the rationale.
