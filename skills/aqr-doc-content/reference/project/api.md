# API reference content criteria (libraries)

Purpose: a consumer reads it and can use the library correctly without reading code or design docs. State the public surface as a reference, not as a design — what each symbol is and how to call it, not why it was designed that way.

The design counterpart — why the API is shaped this way, stability and versioning decisions — lives in `api_design.md`; its content criteria are in `reference/project/api_design.md`.

Content:

1. **Reference** — for each public symbol (function, type, trait, class, configuration surface): signature, behavior in one or two sentences, error cases. Link to the design doc for rationale; do not reproduce it.
2. **Stability** — the stability tag (`stable` / `experimental` / `deprecated`) and the version where it last changed.
3. **Usage examples** — the canonical ways consumers use this surface, as runnable examples or recipes. Include the integration shape (initialization, lifecycle, teardown) when it is non-trivial.
4. **Supported consumer matrix** — the OS, language runtime, and dependency versions the library commits to support.

Constraints: target a doc a consumer can scan. Generated reference is fine if it meets these criteria; hand-write the parts generation cannot cover (usage examples, integration shape).
