# Usage scenarios content criteria

## Purpose

A reviewer reads it and understands the concrete situations the project must handle, without reading code. Scenarios bridge the mission (what the project is for) and the design (how it is built) by pinning down observable behavior the system must produce.

For a system repo: user-facing scenarios. For a library repo: integration scenarios, told from the consumer's perspective. Omit for a noncode repo.

## Content

1. **Overview** — one paragraph: the user or consumer population and the range of situations this doc covers.
2. **Scenarios** — one subsection per scenario. Each: a short title, a one-paragraph description of the situation, what the user or consumer does, and what the system or library must do in response — described as observable behavior, not implementation.

Frame each scenario around a user or consumer and a goal; state the situation and the required response, not how the system achieves it.

## Constraints

- Scenarios must be concrete, not abstract feature lists. "A researcher uploads a 500 MB CSV and expects row-level validation errors within 30 seconds" is a scenario; "support large files" is not. For a library: "A Python service calls `retry(fn, { attempts: 5 })` from an async context and expects no event-loop blocking" is a scenario; "support async" is not.
- Describe observable behavior, not implementation — "the system returns invalid rows within 30 seconds," not "the system spawns a worker that streams the file." For a library: "the library returns a `RetryResult` with the last error after 5 attempts within the configured backoff," not "the library loops with `await sleep(...)`."
- One scenario per situation; keep divergent responses as separate scenarios so each is testable.
