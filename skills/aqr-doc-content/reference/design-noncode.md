# Noncode design content criteria

Criteria for design docs in a noncode repo — docs are the repo's primary output, not a description of code. The design doc is a proposal: it states a problem, compares alternatives, and records a decision. There is no implementation in this repo to describe; if the design will be implemented, it happens elsewhere.

## 1. Contents

- **Summary** — what this proposal covers and explicitly what it does not.
- **Problem statement** — what is broken or missing today, concretely. Reference prior work, incidents, or external context.
- **Goals and non-goals** — what a good solution must achieve, and what is explicitly out of scope for this proposal.
- **Proposed design** — the recommended solution, described at the conceptual level. No reference to code in this repo (there is none); if the design implies code shapes, describe them as conceptual structures and stop there. State the intended reader's takeaway in one sentence before the detail.
- **Alternatives compared** — the bulk of the doc. For each alternative considered:
    - **What it is** — one paragraph.
    - **Trade-offs** — what it does well and where it falls short, measured against the goals above.
    - **Why not** — the single reason that ruled it out, if there is one.
- **Decisions and rationale** — the decision records that emerged from comparing alternatives, one per entry; format in §2.3.
- **Open questions** — what is unresolved and what would unblock it. Say "deferred" with a trigger condition rather than "TBD".
- **Forward reference** — where this design will be implemented, if anywhere. Often an external repo, a follow-up system repo, or a downstream team. Link the issue, repo, or team; if no implementation is planned, say so.

## 2. Design criteria

### 2.1 Universal design criteria

- Reviewable without leaving the doc; a reader can understand the proposal and the decision from the text alone.
- Explicit boundaries — goals and non-goals are stated, and the proposed design stays within them.
- Conceptual only; no fake implementation details invented to make the doc look like a system-repo design. If code shapes are implied, describe them at the level of structures and stop.
- Be concise. Do not write what could be a list as prose — describe what and why, not how it would be implemented.
- Alternatives are real: each must be plausibly something a reasonable person would propose. Strawman alternatives weaken the decision.

### 2.2 Recording design decisions

Record each significant decision as a short entry with three fields:

- **Context** — the forces that forced the choice.
- **Current decision** — what was chosen, in one line.
- **Alternatives** — options considered (including past decisions since reversed) and why they were not picked.

One decision per entry. When a decision is reversed, update Current decision and move the old one into Alternatives with a one-line reason.
