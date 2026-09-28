# Status content criteria

## 1. Purpose

A reader or agent returning after time away understands where the project stands, what to do next, and which key choices were made.

## 2. Content

1. **Current stage** — what is done and what is in flight now, one line per item.
2. **Next tasks** — the queued work, one line each, in the order it will be tackled. A target date may be added when one exists.
3. **Decisions log** — project-level decisions (scope, priorities, direction), each with a one-line rationale and date. Example: "2026-03: deferred multi-tenant isolation to v2 — single-tenant ships first to meet the pilot deadline; revisit when a second tenant is signed."

## 3. Constraints

- Keep it current — a stale status misleads more than no status.
- Decisions stay one line each; the full reasoning goes into the matching design doc's decision records.
- Long-term direction lives in mission.md — do not duplicate it here.
