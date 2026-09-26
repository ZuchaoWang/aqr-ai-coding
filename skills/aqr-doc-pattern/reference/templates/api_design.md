# API design content criteria (libraries)

## 1. Purpose

A consumer or contributor reads it and understands the library's public API — what each symbol is, how to use it, and why the API is shaped this way.

## 2. Content

1. **API signatures** — for each public symbol (function, type, trait, class, configuration surface): signature, behavior in one or two sentences, error cases.
2. **Usage examples** — the canonical ways consumers use this surface, as runnable examples or recipes. Include the integration shape (initialization, lifecycle, teardown) when it is non-trivial.
3. **Design decisions** — decision records for significant API choices (what was chosen, alternatives considered, why).

## 3. Constraints

Target a doc a consumer can scan. State the contract (parameters, return type, error cases); leave the implementation in code.
