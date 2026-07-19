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

## 2. Roadmap

Purpose: a reviewer reads it and understands where the library is heading, what is in flight now, and what comes next.

Sections:

1. **Vision** — the long-term direction; where the library is heading qualitatively.
2. **Now** — the current milestone; what is in flight.
3. **Next** — the milestone queued after Now.
4. **Later** — milestones beyond Next, stated at a coarser grain.
5. **Decisions log** — key decisions that shaped the roadmap, each with a one-line rationale and date. Example: "2026-03: deferred streaming API to v2 — batch API ships first to meet the pilot consumer's deadline; revisit when a second consumer needs streaming."

Constraints: reference work by name rather than re-describing it. For a date-driven library, swap Now/Next/Later for dated milestones (name, target date, what is included). Keep it current — a stale roadmap misleads more than no roadmap.

## 3. Concepts

The vocabulary the library exposes — the terms consumers will meet in the API, docs, and error messages. Concepts considered but not adopted belong in background, not here. For each concept:

- **Definition** — one or two sentences precise enough that two contributors would agree on it.
- **Used here** — where it shows up (a public type, function, error code, or doc term). If it has no use site, it belongs in background, not here.

This is the active vocabulary, not a dictionary — list only what a reader needs to understand the rest of the docs and the API.

## 4. API reference

Purpose: a consumer reads it and can use the library correctly without reading code or design docs. State the public surface as a reference, not as a design — what each symbol is and how to call it, not why it was designed that way.

Content:

1. **Reference** — for each public symbol (function, type, trait, class, configuration surface): signature, behavior in one or two sentences, error cases. Link to the design doc for rationale; do not reproduce it.
2. **Stability** — the stability tag (`stable` / `experimental` / `deprecated`) and the version where it last changed.
3. **Usage examples** — the canonical ways consumers use this surface, as runnable examples or recipes. Include the integration shape (initialization, lifecycle, teardown) when it is non-trivial.
4. **Supported consumer matrix** — the OS, language runtime, and dependency versions the library commits to support.

Constraints: target a doc a consumer can scan. Generated reference is fine if it meets these criteria; hand-write the parts generation cannot cover (usage examples, integration shape).
