# API design content criteria (libraries)

Purpose: a consumer or contributor reads it and understands the library's public API — what each symbol is, how to use it, and why the API is shaped this way.

Content:

1. **API signatures** — for each public symbol (function, type, trait, class, configuration surface): signature, behavior in one or two sentences, error cases.
2. **Usage examples** — the canonical ways consumers use this surface, as runnable examples or recipes. Include the integration shape (initialization, lifecycle, teardown) when it is non-trivial.
3. **Design decisions** — decision records for significant API choices (what was chosen, alternatives considered, why). Use the same decision-recording format as the system design reference.

Constraints: target a doc a consumer can scan. Generated signatures are acceptable; hand-write the usage examples and design decisions.
