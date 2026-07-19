# Noncode project doc content criteria

Criteria for the project-level docs of a noncode repo. The repo's primary output is documentation, not code, so the framing shifts from "what this system does for its users" to "what knowledge or decisions this repo captures, and who reads them".

## 1. Mission

Purpose: a new reader reads it and immediately understands what this repo captures, who it is for, and why it exists as a standalone repo rather than a folder inside another project.

Sections:

1. **Goal** — one paragraph: what knowledge, decisions, or proposals this repo holds, and what a reader can find here that they could not find elsewhere.
2. **Problem statement** — why this repo exists. Concretely: what was scattered, missing, or unrecoverable before that motivated collecting it here.
3. **Scope** — which topics, projects, or decisions the repo is the source of record for; what is explicitly out of scope and where it belongs instead.
4. **Audience** — the kinds of reader (e.g. "engineer joining a project that uses this design", "decision-maker evaluating options", "future maintainer reversing a decision"). State what each persona is assumed to know and what they are looking for; this shapes how docs are structured and indexed.
5. **Stakeholders** — who maintains the repo, who reads it, who decides what gets included.

Constraints: target ~1 page. Scope and audience must be explicit — a reader should know whether they are the intended audience and whether a topic belongs here. Prefer concrete audience personas over generic "anyone interested". Revise scope when it shifts; ambiguous scope creates drift.

## 2. Roadmap

Purpose: a maintainer reads it and knows what decisions are still open, what docs are pending, and in roughly what order.

Sections:

1. **Open decisions** — unresolved questions the repo is meant to settle, with the trigger that would unblock each.
2. **Pending docs** — docs known to be missing, in intended order.
3. **Decisions log** — key decisions that shaped the repo's scope or direction, each with a one-line rationale and date.

Constraints: reference work by name rather than re-describing it. Keep it current — a stale roadmap misleads more than no roadmap.
