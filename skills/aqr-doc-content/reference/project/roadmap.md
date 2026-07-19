# Roadmap content criteria

Purpose: a reviewer reads it and understands where the project is heading, what is in flight now, and what comes next.

Sections (system and library repos):

1. **Vision** — the long-term direction; where the project is heading qualitatively.
2. **Now** — the current milestone; what is in flight.
3. **Next** — the milestone queued after Now.
4. **Later** — milestones beyond Next, stated at a coarser grain.
5. **Decisions log** — key decisions that shaped the roadmap, each with a one-line rationale and date. Example: "2026-03: deferred multi-tenant isolation to v2 — single-tenant ships first to meet the pilot deadline; revisit when a second tenant is signed."

For a noncode repo, swap the milestone sections for:

1. **Open decisions** — unresolved questions the repo is meant to settle, with the trigger that would unblock each. Say "deferred" with a trigger condition rather than "TBD".
2. **Pending docs** — docs known to be missing, in intended order.
3. **Decisions log** — key decisions that shaped the repo's scope or direction, each with a one-line rationale and date.

Constraints: reference work by name rather than re-describing it. For a date-driven project, swap Now/Next/Later for dated milestones (name, target date, what is included). Keep it current — a stale roadmap misleads more than no roadmap.
