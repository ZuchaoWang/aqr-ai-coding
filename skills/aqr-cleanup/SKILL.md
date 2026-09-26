---
name: aqr-cleanup
description: Use when manually invoked to clean up an existing code project. Thorough mode checks code/doc consistency, addresses code problems and quality risks, flags design-level complexity, and verifies fixes. Quick mode fixes only typos, stale references, and simple inconsistencies with no complexity analysis or refactoring.
disable-model-invocation: true
---

# aqr-cleanup

Manual-only cleanup pass over an existing code project — a repo whose primary output is code, with docs that describe that code. A noncode repo (docs are the primary output) is out of scope: there is no code to clean, so review it with the normal doc criteria instead. Do not auto-invoke; run only when explicitly named. The user specifies the mode in their request (e.g. "aqr-cleanup quick mode"). Default to thorough if unspecified.

After every non-trivial fix, verify it: run the project's tests if available, otherwise exercise the affected paths by hand. Stop and report if a fix cannot be verified.

## Thorough mode

Full cleanup pass with three-phase workflow and clarification checkpoints.

Issues in scope, found by reading docs and code together:

- **Code/doc inconsistencies** - module responsibilities, interface contracts, file paths, names, signatures, documented vs. actual behavior.
- **Code problems** - duplication that can be extracted, unclear names, dead code and unreachable branches, unnecessary complexity with a simpler equivalent, necessary complexity that can be decomposed into separate functions or classes, non-idiomatic code or best-practice violations, inconsistencies with surrounding code.
- **Quality risks** - likely bugs and unhandled edge cases, performance hotspots, security exposures.
- **Design-level complexity** - code whose complexity is the root cause of repeated bugs or makes safe change impossible, and whose real fix is a high-level redesign rather than a local refactor. Surface with a recommended redesign direction; do not attempt the redesign as part of the cleanup.

### 1. Investigate and fix the obvious

Catalog issues from all categories above. Fix the obvious ones immediately - where the correct fix is clear and low-risk (typos, stale references, simple renames, isolated dead code, mechanical refactors, clear bug fixes such as a missing null check). Where doc and code disagree, fix the side that does not match the intended design; consult git history if the intent is unclear. Smallest change that cleanly fixes each. No speculative refactors. Verify each fix.

Defer anything ambiguous to phase 2.

### 2. Report items that need clarification

For each deferred item:

- describe the problem and where it lives
- state the options and the trade-off
- name the side you would fix if forced to choose

Pause for the user to clarify intent.

### 3. Fix the clarified items

Apply the confirmed fixes, then verify each.

## Quick mode

Fast pass for obvious fixes only. No complexity analysis, no quality-risk hunting, no design-level review, no speculative refactors.

Issues in scope:

- Typos in code, comments, or docs.
- Stale references - wrong file paths, outdated names, broken links.
- Stale docs where the code reflects the intended behavior - fix the doc to match.
- Isolated dead code that is obviously unused.
- Mechanical corrections - missing punctuation, wrong casing, formatting slips.

Anything not an obvious fix is skipped and noted in the report, not deferred for clarification.

### 1. Scan and fix

Scan for issues in the scope above. Fix each one immediately - smallest change that cleanly fixes it. Verify each fix. No clarification phase; anything ambiguous is skipped.

## Report

Summarize changes by file with reason. In thorough mode, distinguish outright fixes (phase 1) from clarified fixes (phase 3). In quick mode, list skipped items separately. Flag anything still open.
