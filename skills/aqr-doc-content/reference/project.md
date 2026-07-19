# Project doc content criteria

Criteria for the project-level docs — mission, roadmap, scenarios, tech stack, concepts, and (for libraries) the API reference. These are lighter than design docs — they state what and why at the project level, not how. Apply the sections that fit the project; omit those that don't. Per-repo-type variants are noted inline.

## 1. Mission

Purpose: a new team member reads it and immediately understands what this project is for, who it serves, and (where relevant) what it replaces.

Sections:

1. **Goal** — one paragraph: what this project does and for whom. For a library, also state what consumers can accomplish with it that they could not (or could not as easily) before. For a noncode repo, state what knowledge, decisions, or proposals it holds and what a reader can find here that they could not find elsewhere.
2. **Problem statement** — what is broken or missing today, concretely. Reference prior work, incidents, or external context. For a library, reference the prior libraries, manual approaches, or workarounds it replaces. For a noncode repo, reference what was scattered, missing, or unrecoverable before.
3. **Scope** — in scope and out of scope for the project as a whole. Revise when scope shifts; ambiguous scope creates drift. For a noncode repo, also name where out-of-scope topics belong (another repo, a folder, etc.).
4. **Stakeholders** — who maintains the project, who depends on or reads it, who decides priorities.

Constraints: target ~1 page. Scope must be explicit.

## 2. Usage scenarios

Purpose: a reviewer reads it and understands the concrete situations the project must handle, without reading code. Scenarios bridge the mission (what the project is for) and the design (how it is built) by pinning down observable behavior the system must produce.

For a system repo: user-facing scenarios. For a library repo: integration scenarios, told from the consumer's perspective. Omit for a noncode repo.

Sections:

1. **Overview** — one paragraph: the user or consumer population and the range of situations this doc covers.
2. **Scenarios** — one subsection per scenario. Each: a short title, a one-paragraph description of the situation, what the user or consumer does, and what the system or library must do in response — described as observable behavior, not implementation.

Frame each scenario around a user or consumer and a goal; state the situation and the required response, not how the system achieves it.

Constraints:

- Scenarios must be concrete, not abstract feature lists. "A researcher uploads a 500 MB CSV and expects row-level validation errors within 30 seconds" is a scenario; "support large files" is not. For a library: "A Python service calls `retry(fn, { attempts: 5 })` from an async context and expects no event-loop blocking" is a scenario; "support async" is not.
- Describe observable behavior, not implementation — "the system returns invalid rows within 30 seconds," not "the system spawns a worker that streams the file." For a library: "the library returns a `RetryResult` with the last error after 5 attempts within the configured backoff," not "the library loops with `await sleep(...)`."
- One scenario per situation; keep divergent responses as separate scenarios so each is testable.

## 3. Roadmap

Purpose: a reviewer reads it and understands where the project is heading, what is in flight now, and what comes next.

Sections (system and library repos):

1. **Vision** — the long-term direction; where the project is heading qualitatively.
2. **Now** — the current milestone; what is in flight.
3. **Next** — the milestone queued after Now.
4. **Later** — milestones beyond Next, stated at a coarser grain.
5. **Decisions log** — key decisions that shaped the roadmap, each with a one-line rationale and date. Example: "2026-03: deferred multi-tenant isolation to v2 — single-tenant ships first to meet the pilot deadline; revisit when a second tenant is signed."

Constraints: reference work by name rather than re-describing it. For a date-driven project, swap Now/Next/Later for dated milestones (name, target date, what is included). Keep it current — a stale roadmap misleads more than no roadmap.

## 4. Tech stack

Purpose: a reviewer reads it and understands what tools are in use and why those choices were made. Omit for a noncode repo.

Sections:

1. **Languages and runtimes** — one line per language: version, where the pin lives.
2. **Frameworks and libraries** — one line per key dependency: what it does here, why it was chosen over alternatives. When a library was chosen over custom code (library-first), say so and name the custom code it replaces — e.g. "cockatiel — retry logic, chosen over hand-rolled retry."
3. **Toolchain** — linters, formatters, type checkers, test runners, build tools.
4. **Rationale note** — any non-obvious choice that would confuse a reader without explanation.

Constraints: target ~1 page. If a tool is standard and the reason is obvious ("pytest because it's the Python default"), omit the rationale.

## 5. Concepts

The domain concepts the project actively uses. Concepts considered but not adopted belong in background, not here. For a library, this is the API vocabulary — the terms consumers will meet in the API, docs, and error messages.

For each concept:

- **Definition** — one or two sentences precise enough that two team members would agree on it.
- **Used here** — where it shows up (a component, data model, algorithm, user-facing term, or — for a library — a public type, function, or error code). If it has no use site, it belongs in background, not here.

This is the active vocabulary, not a dictionary — list only what a reader needs to understand the rest of the docs.

## 6. API reference (libraries)

Purpose: a consumer reads it and can use the library correctly without reading code or design docs. State the public surface as a reference, not as a design — what each symbol is and how to call it, not why it was designed that way.

The design counterpart — why the API is shaped this way, stability and versioning decisions — lives in `api_design.md` alongside this file; its content criteria are the library overlay in `reference/code-design.md`.

Content:

1. **Reference** — for each public symbol (function, type, trait, class, configuration surface): signature, behavior in one or two sentences, error cases. Link to the design doc for rationale; do not reproduce it.
2. **Stability** — the stability tag (`stable` / `experimental` / `deprecated`) and the version where it last changed.
3. **Usage examples** — the canonical ways consumers use this surface, as runnable examples or recipes. Include the integration shape (initialization, lifecycle, teardown) when it is non-trivial.
4. **Supported consumer matrix** — the OS, language runtime, and dependency versions the library commits to support.

Constraints: target a doc a consumer can scan. Generated reference is fine if it meets these criteria; hand-write the parts generation cannot cover (usage examples, integration shape).
