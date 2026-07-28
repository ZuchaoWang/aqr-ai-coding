---
name: aqr-cleanup
description: Use when manually invoked to clean up an existing project — checks code/doc consistency, addresses code problems and quality risks (bugs, performance, security), flags design-level complexity that needs high-level redesign, and verifies fixes do not break behavior.
disable-model-invocation: true
---

# aqr-cleanup

Manual-only cleanup pass over an existing project. Do not auto-invoke; run only when explicitly named.

Issues in scope, found by reading docs and code together:

- **Code/doc inconsistencies** — module responsibilities, interface contracts, file paths, names, signatures, documented vs. actual behavior.
- **Code problems** — duplication that can be extracted, unclear names, dead code and unreachable branches, unnecessary complexity with a simpler equivalent, necessary complexity that can be decomposed into separate functions or classes, non-idiomatic code or best-practice violations, inconsistencies with surrounding code.
- **Quality risks** — likely bugs and unhandled edge cases, performance hotspots, security exposures.
- **Design-level complexity** — code whose complexity is the root cause of repeated bugs or makes safe change impossible, and whose real fix is a high-level redesign rather than a local refactor. Surface these in phase 2 with a recommended redesign direction; do not attempt the redesign as part of the cleanup.

After every non-trivial fix, verify it: run the project's tests if available, otherwise exercise the affected paths by hand. Refactors and consistency fixes should leave behavior unchanged; bug, performance, or security fixes should produce the intended behavior. Stop and report if a fix cannot be verified.

## Steps

The cleanup involves three phases, in order:

### 1. Investigate and Fix the Obvious

Catalog issues from all categories above. Fix the obvious ones immediately — where the correct fix is clear and low-risk (typos, stale references, simple renames, isolated dead code, mechanical refactors, clear bug fixes such as a missing null check). Where doc and code disagree, fix the side that does not match the intended design; consult git history if the intent is unclear. Smallest change that cleanly fixes each. No speculative refactors. Verify each fix per the rule above.

Defer anything ambiguous to phase 2.

### 2. Report Items That Need Clarification

For each deferred item:

- describe the problem and where it lives
- state the options and the trade-off
- name the side you would fix if forced to choose

Pause for the user to clarify intent.

### 3. Fix the Clarified Items

Apply the confirmed fixes, then verify per the rule above.

## Report

Summarize changes by file with reason. Distinguish outright fixes (phase 1) from clarified fixes (phase 3). Flag anything still open.
