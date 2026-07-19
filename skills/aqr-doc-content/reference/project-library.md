# Library project doc content criteria

Criteria for the project-level docs of a library code repo. The library is published for other repos to depend on, so the framing shifts from "end users of a system" to "consumers integrating the library". These docs are lighter than design docs — they state what and why at the project level, not how.

## 1. Mission

Purpose: a new contributor reads it and immediately understands what the library does, who depends on it, and what it replaces.

Sections:

1. **Goal** — one paragraph: what the library does, for whom, and what consumers can accomplish with it that they could not (or could not as easily) before.
2. **Problem statement** — what is missing or painful today, concretely. Reference prior libraries, manual approaches, or workarounds this library replaces.
3. **Scope** — in scope and out of scope for the library as a whole. Revise when scope shifts; ambiguous scope creates drift.
4. **Stakeholders** — who maintains the library, who depends on it (named consumers or consumer classes), who decides priorities.

Constraints: target ~1 page. Scope must be explicit.

## 2. Integration scenarios

Purpose: a reviewer reads it and understands the concrete consumer situations the library must handle, without reading code. Scenarios bridge the mission (what the library is for) and the design (how the public surface is built) by pinning down observable behavior from the consumer's point of view.

Sections:

1. **Overview** — one paragraph: the consumer population and the range of integration situations this doc covers.
2. **Scenarios** — one subsection per scenario. Each: a short title, a one-paragraph description of the integration situation, what the consumer does, and what the library must do in response — described as observable behavior, not implementation.

Frame each scenario around a consumer and a goal; state the situation and the required response, not how the library achieves it.

Constraints:

- Scenarios must be concrete, not abstract feature lists. "A Python service calls `retry(fn, { attempts: 5 })` from an async context and expects no event-loop blocking" is a scenario; "support async" is not.
- Describe observable behavior, not implementation — "the library returns a `RetryResult` with the last error after 5 attempts within the configured backoff," not "the library loops with `await sleep(...)`."
- One scenario per situation; keep divergent responses as separate scenarios so each is testable.

## 3. Roadmap

Purpose: a reviewer reads it and understands where the library is heading, what is in flight now, and what comes next.

Sections:

1. **Vision** — the long-term direction; where the library is heading qualitatively.
2. **Now** — the current milestone; what is in flight.
3. **Next** — the milestone queued after Now.
4. **Later** — milestones beyond Next, stated at a coarser grain.
5. **Decisions log** — key decisions that shaped the roadmap, each with a one-line rationale and date. Example: "2026-03: deferred streaming API to v2 — batch API ships first to meet the pilot consumer's deadline; revisit when a second consumer needs streaming."

Constraints: reference work by name rather than re-describing it. For a date-driven library, swap Now/Next/Later for dated milestones (name, target date, what is included). Keep it current — a stale roadmap misleads more than no roadmap.

## 4. Tech stack

Purpose: a reviewer reads it and understands what tools are in use and why those choices were made.

Sections:

1. **Languages and runtimes** — one line per language: version, where the pin lives.
2. **Frameworks and libraries** — one line per key dependency: what it does here, why it was chosen over alternatives. When a library was chosen over custom code (library-first), say so and name the custom code it replaces — e.g. "cockatiel — retry logic, chosen over hand-rolled retry."
3. **Toolchain** — linters, formatters, type checkers, test runners, build tools.
4. **Supported consumer matrix** — the OS, language runtime, and dependency versions the library commits to support. This is a project-level promise; per-version compatibility details live in design docs.
5. **Rationale note** — any non-obvious choice that would confuse a reader without explanation.

Constraints: target ~1 page. If a tool is standard and the reason is obvious ("pytest because it's the Python default"), omit the rationale.

## 5. Concepts

The vocabulary the library exposes — the terms consumers will meet in the API, docs, and error messages. Concepts considered but not adopted belong in background, not here. For each concept:

- **Definition** — one or two sentences precise enough that two contributors would agree on it.
- **Used here** — where it shows up (a public type, function, error code, or doc term). If it has no use site, it belongs in background, not here.

This is the active vocabulary, not a dictionary — list only what a reader needs to understand the rest of the docs and the API.

## 6. Publishing and distribution

Purpose: state at the project level how the library reaches consumers and what guarantees come with that. Per-package publishing details live in `architecture/publish.md`; this section names the policy.

Content:

1. **Distribution channels** — where the library is published (registry, package manager, git tag, etc.) and how consumers obtain it.
2. **Versioning policy** — the scheme in use (e.g. semver) and what counts as a breaking change.
3. **Support windows** — which published versions receive fixes, and for how long.
4. **Deprecation cycle** — how a public surface gets deprecated and eventually removed; the notice period consumers can rely on.

Constraints: keep this section policy-level; link the detailed file rather than duplicating it.
