# API design content criteria

Applies to any public API surface: a library's public symbols, a pure data API (network endpoints serving data), or a website's server-side API — the endpoints a browser frontend calls.

## 1. Purpose

A consumer or contributor reads it and understands the project's public API — what each symbol or endpoint is, how to use it, and why the API is shaped this way.

## 2. Content

1. **Signatures** — for a library API, each public symbol (function, type, trait, class, configuration surface): signature, behavior in one or two sentences, error cases.
2. **Endpoints** — for a network API, the endpoints grouped by resource: for each, method and path, request fields, response fields, and error shape; state authentication requirements when present. This covers a pure data API and a website's server-side API — the endpoints behind the frontend.
3. **Usage examples** — the canonical ways consumers use this surface: for a library, runnable examples or recipes; for a network API, sample requests and responses. Include the integration shape (initialization, lifecycle, teardown) when it is non-trivial.
4. **Design decisions** — decision records for significant API choices (what was chosen, alternatives considered, why).

## 3. Constraints

Target a doc a consumer can scan. State the contract (parameters and fields, return or response, error cases); leave the implementation in code.
