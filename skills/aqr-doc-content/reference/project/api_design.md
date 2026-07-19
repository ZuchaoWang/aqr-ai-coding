# API design content criteria (libraries)

Criteria for the library API design doc — `api_design.md`. The library is published for other repos to depend on, so the public API surface is the contract consumers depend on. Document it more fully than for an internal module: why the surface is shaped this way, and the stability and versioning decisions behind it.

The reference counterpart — what each symbol is and how to call it — lives in `api.md`; its content criteria are in `reference/project/api.md`.

Content:

- **Public API surface** — for each public symbol (function, type, trait, class, configuration surface):
    - **Signature and behavior contract** — inputs, outputs, side effects, error cases. The conceptual behavior, not a re-print of code.
    - **Stability tag** — `stable`, `experimental`, or `deprecated`, with the version where the tag last changed.
    - **Semver implications** — what kinds of changes to this symbol would require a major, minor, or patch release. State this even for `experimental` symbols (typically: "any change, no notice").
- **Usage examples and integration patterns** — the canonical ways consumers use this surface, as runnable examples or recipes. Include the integration shape (initialization, lifecycle, teardown) when it is non-trivial.
- **Versioning, backwards compatibility, and deprecation policy** — what the library guarantees across versions; how breaking changes are introduced (deprecation cycle, support windows, migration aids). If a published policy lives elsewhere (README, CHANGELOG), reference it; do not duplicate.
- **Stability testing** — how the public surface is tested for stability: the consumer-relevant matrix (OS, language runtime, supported dependency versions) and any compatibility tests against prior published versions.
