---
name: aqr-cleanup
description: Use when manually invoked to clean up an existing project — checks code/doc consistency, refactors code problems, and verifies the refactor does not break behavior.
disable-model-invocation: true
---

# aqr-cleanup

Manual-only cleanup pass over an existing project. Do not auto-invoke; run only when explicitly named.

Two kinds of issues are in scope, found by reading docs and code together:

- **Code/doc inconsistencies** — module responsibilities, interface contracts, file paths, names, signatures, documented vs. actual behavior.
- **Code problems** — duplication that can be extracted, unclear names, dead code and unreachable branches, unnecessary complexity with a simpler equivalent, necessary complexity that can be decomposed into separate functions or classes, inconsistencies with surrounding code.

After every non-trivial fix, confirm behavior is unchanged: run the project's tests if available, otherwise exercise the affected paths by hand. Stop and report if a fix cannot be verified.

## Steps

The cleanup involves three phases, in order:

### 1. Investigate and Fix the Obvious

Catalog issues from both categories above. Fix the obvious ones immediately — typos, stale references, simple renames, isolated dead code, mechanical refactors. Where doc and code disagree, fix the side that does not match the intended design; consult git history if the intent is unclear. Smallest change that cleanly fixes each. No speculative refactors. Verify each fix per the rule above.

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
