# Skills Consolidation Implementation Plan

> **For agentic workers:** REQUIRED SUB-SKILL: Use superpowers:subagent-driven-development (recommended) or superpowers:executing-plans to implement this plan task-by-task. Steps use checkbox (`- [ ]`) syntax for tracking.

**Goal:** Restructure the skills tree to 5 flat skills — consolidated `aqr-code-pattern` and `aqr-doc-pattern`, new `aqr-presentation`, scope-corrected `aqr-cleanup`, unchanged `aqr-project-principle` — and slim the system/frontend design-doc criteria to component-level depth.

**Architecture:** Pure moves (`git mv`) for verbatim content, then hand-written index `SKILL.md` files that route into subfoldered references. Two template files are rewritten to the slimmed criteria. README.md and CLAUDE.md are updated to match. Spec: `docs/superpowers/specs/2026-09-26-skill-consolidation-design.md`.

**Tech Stack:** Markdown only. Verification is by inspection (`find`/`grep` + reading), per this repo's CLAUDE.md — there is no test suite.

## Global Constraints

- Doc-only repo: no executable code anywhere.
- Markdown conventions (from CLAUDE.md): files start with a `# Title`; reference docs use numbered headings and `-` bullets; no `---` horizontal separators; no `Status:` header.
- Every `SKILL.md` starts with YAML frontmatter containing `name`, `description`, optional `disable-model-invocation`.
- Verbatim-move policy: moved files are unchanged except edits stated explicitly in the plan.
- Retired names — `aqr-code-criteria`, `aqr-design-system`, `aqr-frontend-patterns`, `aqr-doc-blueprint`, `aqr-doc-content`, `aqr-style-rules` — must not appear anywhere under `skills/`, `agents/`, `CLAUDE.md`, or `README.md` at the end. (The spec and this plan under `docs/` mention them by design; the final grep excludes `docs/`.)
- Commit messages use the repo's `topic: summary` style (`skills: ...`, `docs: ...`).

---

### Task 1: Flatten the tree with git mv

**Files:**
- Move: all files from `skills/general/` and `skills/opinionated/` into the new flat layout (mapping below)
- Delete: the five retired `SKILL.md` files

**Interfaces:**
- Produces: the exact directory skeleton all later tasks fill in:
  `skills/aqr-code-pattern/` (SKILL.md missing until Task 2; `reference/{frontend_patterns,design_system}.md`, `reference/styles/{code_python,code_javascript,notebooks}.md`),
  `skills/aqr-doc-pattern/` (SKILL.md missing until Task 3; `reference/blueprint/{system,library,noncode}.md`, `reference/templates/*.md` ×11, `reference/styles/doc_style.md`),
  `skills/aqr-presentation/` (SKILL.md missing until Task 6; `reference/presentation.md`),
  `skills/aqr-cleanup/SKILL.md`, `skills/aqr-project-principle/SKILL.md`.

- [ ] **Step 1: Create the new directories**

```bash
mkdir -p skills/aqr-code-pattern/reference/styles \
         skills/aqr-doc-pattern/reference/blueprint \
         skills/aqr-doc-pattern/reference/templates \
         skills/aqr-doc-pattern/reference/styles \
         skills/aqr-presentation/reference
```

- [ ] **Step 2: Move files with git mv (history preserved)**

```bash
git mv skills/opinionated/aqr-frontend-patterns/SKILL.md skills/aqr-code-pattern/reference/frontend_patterns.md
git mv skills/general/aqr-design-system/SKILL.md skills/aqr-code-pattern/reference/design_system.md
git mv skills/opinionated/aqr-style-rules/reference/code-python.md skills/aqr-code-pattern/reference/styles/code_python.md
git mv skills/opinionated/aqr-style-rules/reference/code-javascript.md skills/aqr-code-pattern/reference/styles/code_javascript.md
git mv skills/opinionated/aqr-style-rules/reference/notebooks.md skills/aqr-code-pattern/reference/styles/notebooks.md
git mv skills/opinionated/aqr-doc-blueprint/reference/system.md skills/aqr-doc-pattern/reference/blueprint/system.md
git mv skills/opinionated/aqr-doc-blueprint/reference/library.md skills/aqr-doc-pattern/reference/blueprint/library.md
git mv skills/opinionated/aqr-doc-blueprint/reference/noncode.md skills/aqr-doc-pattern/reference/blueprint/noncode.md
for f in system_design frontend_design ui_design api_design mission usage_scenarios roadmap tech_stack concepts research dataset; do
  git mv "skills/opinionated/aqr-doc-content/reference/$f.md" "skills/aqr-doc-pattern/reference/templates/$f.md"
done
git mv skills/opinionated/aqr-style-rules/reference/documentation.md skills/aqr-doc-pattern/reference/styles/doc_style.md
git mv skills/opinionated/aqr-style-rules/reference/presentation.md skills/aqr-presentation/reference/presentation.md
git mv skills/general/aqr-cleanup skills/aqr-cleanup
git mv skills/general/aqr-project-principle skills/aqr-project-principle
```

- [ ] **Step 3: Delete the retired SKILL.md files and empty directories**

```bash
git rm -q skills/general/aqr-code-criteria/SKILL.md \
          skills/opinionated/aqr-doc-blueprint/SKILL.md \
          skills/opinionated/aqr-doc-content/SKILL.md \
          skills/opinionated/aqr-style-rules/SKILL.md
find skills -type d -empty -delete
```

- [ ] **Step 4: Verify the moved tree**

Run: `find skills -type f | sort`
Expected output (exactly this, nothing else):

```
skills/aqr-cleanup/SKILL.md
skills/aqr-code-pattern/reference/design_system.md
skills/aqr-code-pattern/reference/frontend_patterns.md
skills/aqr-code-pattern/reference/styles/code_javascript.md
skills/aqr-code-pattern/reference/styles/code_python.md
skills/aqr-code-pattern/reference/styles/notebooks.md
skills/aqr-doc-pattern/reference/blueprint/library.md
skills/aqr-doc-pattern/reference/blueprint/noncode.md
skills/aqr-doc-pattern/reference/blueprint/system.md
skills/aqr-doc-pattern/reference/styles/doc_style.md
skills/aqr-doc-pattern/reference/templates/api_design.md
skills/aqr-doc-pattern/reference/templates/concepts.md
skills/aqr-doc-pattern/reference/templates/dataset.md
skills/aqr-doc-pattern/reference/templates/frontend_design.md
skills/aqr-doc-pattern/reference/templates/mission.md
skills/aqr-doc-pattern/reference/templates/research.md
skills/aqr-doc-pattern/reference/templates/roadmap.md
skills/aqr-doc-pattern/reference/templates/system_design.md
skills/aqr-doc-pattern/reference/templates/tech_stack.md
skills/aqr-doc-pattern/reference/templates/ui_design.md
skills/aqr-doc-pattern/reference/templates/usage_scenarios.md
skills/aqr-presentation/reference/presentation.md
skills/aqr-project-principle/SKILL.md
```

- [ ] **Step 5: Commit**

```bash
git add -A && git commit -m "skills: flatten tree and move references for consolidation"
```

---

### Task 2: aqr-code-pattern — SKILL.md and reference identity fixes

**Files:**
- Create: `skills/aqr-code-pattern/SKILL.md`
- Modify: `skills/aqr-code-pattern/reference/frontend_patterns.md` (strip frontmatter, retitle H1)
- Modify: `skills/aqr-code-pattern/reference/design_system.md` (strip frontmatter, retitle H1)

**Interfaces:**
- Consumes: files moved by Task 1.
- Produces: skill name `aqr-code-pattern` (used by Task 9's CLAUDE.md).

- [ ] **Step 1: Write `skills/aqr-code-pattern/SKILL.md`**

The `## 1.`–`## 10.` body below is the verbatim text of the former `aqr-code-criteria` body.

````markdown
---
name: aqr-code-pattern
description: Universal code quality and design principles plus opinionated stack patterns and styles. Use when writing or reviewing source code or design docs; when implementing, migrating, or auditing UI against a design system; when writing or reviewing frontend code; or when applying this repo's code, test, or notebook style defaults.
disable-model-invocation: false
---

# aqr-code-pattern

Universal code quality and design principles — a floor, not a ceiling, applying regardless of language or stack. Apply at both design time and coding time. The references below layer stack-specific taste on top: architecture patterns and style defaults, opinionated rather than universal. Nothing here is copied into the project.

## Reference index

### Patterns

| Area | What it covers | Reference |
| - | - | - |
| Frontend architecture (React) | Global store vs component state, transforms at the fetch boundary, container/presentational split, component internal ordering, controller extraction | `reference/frontend_patterns.md` |
| Design system | Applying an existing design system to a project: build, extend, restyle, or audit UI | `reference/design_system.md` |

### Styles

Opinionated stack defaults — apply unless the project records a different choice.

| Area | What it covers | Reference |
| - | - | - |
| Code and tests (Python) | `TypedDict` over `dataclass`, `os.path` over `pathlib`, relative imports, naming, plain test functions, exact-equality assertions, ruff / pyright / pytest toolchain, `.python-version` / `pyproject.toml` config, editorconfig | `reference/styles/code_python.md` |
| Code and tests (JavaScript) | `.nvmrc` Node version pin, editorconfig, naming | `reference/styles/code_javascript.md` |
| Notebooks (Jupyter) | kernel pin, required header cells, root-finding boilerplate | `reference/styles/notebooks.md` |

## 1. Decomposition and responsibility

- Keep business logic independent of frameworks, UI, and data access: do not mix business logic into UI components or put database queries in controllers. Separate domain from infrastructure.
- One clear responsibility per unit; if its name needs "and", it is a candidate to split.
- A unit owns its state and hides its internals; callers depend on the contract, not the implementation.
- State lives with the unit that mutates it; lift shared state to the nearest common ancestor.
- Separate what a unit *is* (pure transformation) from what it *does* (owns state and side effects); mixing both is a smell.
- Do not abstract for fewer than three call sites; duplication is cheaper than a premature abstraction.

## 2. Library-first

Prefer an existing library over hand-rolled code for cross-cutting concerns (retry, validation, state management, auth). Custom code is justified only for domain-specific logic, performance-critical paths, or security-sensitive control. Record the chosen library and rationale in the tech-stack doc; the manifest holds the version pin, not the reason.

## 3. Public surface

- The public surface is a contract: cheap to propose, expensive to retract — default to exposing less.
- Type-annotate public APIs; untyped internal helpers are fine. Keep internals out of the public surface.
- If a caller needs to know an internal mechanism, that is a design problem, not a documentation problem.

## 4. Naming

- Named constants, not magic numbers or strings. Names describe what a thing is, not how it is built.
- Avoid dumping-ground names (`utils`, `helpers`, `common`, `shared`); name by domain responsibility so a file's purpose is clear from its name.
- Keep names consistent across the design-to-code boundary unless there is a recorded reason not to.

## 5. Error handling

- Handle errors at the boundary where something meaningful can be done. Do not swallow silently or re-raise without adding information.
- Validate at system boundaries; trust internal code and framework guarantees — do not defend against states that cannot occur.
- Errors are part of the contract: a function states how it fails, and surprising failure modes are bugs. Prefer loud, early failures over silent defaults.

## 6. Comments

- Comments explain why, not what. Omit when the why is obvious; one line when it is not.
- Do not reference issue numbers or PR titles in comments — they rot; they belong in commit messages.

## 7. Secrets and configuration

- Secrets are never hardcoded; read them from environment variables or out-of-tree config.
- Separate per-environment config (read at runtime) from true constants (defined in code). Mixing them makes deployments fragile.

## 8. File and module hygiene

- Do not create empty files (`__init__.py` excepted).
- Past ~300 lines, reconsider a module's scope before adding more.
- Co-locate what changes together; prefer early returns and avoid nesting beyond ~3 levels.

## 9. Testing

- Cover every pure function, and at least one realistic (non-happy-path) test for each public function.
- Test behavior, not implementation; mock slow, nondeterministic, or stateful dependencies and use real implementations for pure logic.
- Boundary tests for system boundaries: invalid input, missing fields, empty collections, off-by-one, large inputs.
- Avoid conceptually duplicated tests — combine minor input variations; split only when the logic path differs. Keep existing tests intact when modifying.

## 10. Before committing

- Run linters, formatters, and the type checker on touched files only — do not reformat the whole tree in an unrelated change.
- A new public function is covered by at least one realistic test.
- If the change affects a public surface or documented contract, update the matching design doc in the same change.
````

- [ ] **Step 2: Fix `reference/frontend_patterns.md` identity**

Delete the frontmatter block (the five lines from the opening `---` through the closing `---`), and replace the H1:

```markdown
# aqr-frontend-patterns
```

→

```markdown
# Frontend patterns (React / Redux Toolkit)
```

All body text below the H1 stays verbatim.

- [ ] **Step 3: Fix `reference/design_system.md` identity**

Delete the frontmatter block (same rule), and replace the H1:

```markdown
# aqr-design-system
```

→

```markdown
# Applying a design system
```

All body text below the H1 stays verbatim.

- [ ] **Step 4: Verify**

Run: `grep -c "^---$" skills/aqr-code-pattern/SKILL.md` — Expected: `2`
Run: `grep -rn "aqr-code-criteria\|aqr-design-system\|aqr-frontend-patterns\|aqr-style-rules" skills/aqr-code-pattern/` — Expected: no output
Run: `head -1 skills/aqr-code-pattern/reference/frontend_patterns.md skills/aqr-code-pattern/reference/design_system.md` — Expected: the new H1 titles, no `---` first line

- [ ] **Step 5: Read `skills/aqr-code-pattern/SKILL.md` end-to-end; confirm the ten criteria sections match the former `aqr-code-criteria` body verbatim.**

- [ ] **Step 6: Commit**

```bash
git add skills/aqr-code-pattern && git commit -m "skills: consolidate code skills into aqr-code-pattern"
```

---

### Task 3: aqr-doc-pattern — SKILL.md and doc_style identity fix

**Files:**
- Create: `skills/aqr-doc-pattern/SKILL.md`
- Modify: `skills/aqr-doc-pattern/reference/styles/doc_style.md` (retitle H1)

**Interfaces:**
- Consumes: files moved by Task 1.
- Produces: skill name `aqr-doc-pattern` (used by Task 9's CLAUDE.md); reference index rows for Task 4/5 filenames.

- [ ] **Step 1: Write `skills/aqr-doc-pattern/SKILL.md`**

````markdown
---
name: aqr-doc-pattern
description: The recommended docs layout plus content criteria for common documentation types and markdown formatting defaults. Use when laying out a project's docs or deciding where a doc belongs; when writing or reviewing any project doc (system design, frontend design, UI design, project, research, dataset); or when applying this repo's markdown style defaults.
disable-model-invocation: false
---

# aqr-doc-pattern

Three layers: the blueprint says what docs a project should have and where (layout only, not content); the templates define what good content looks like for each doc type; the styles set markdown formatting defaults. When writing or reviewing a doc, load the matching reference and apply the sections that fit.

## Blueprint: docs layout by repo type

Two root files route readers and agents into the docs:

```
README.md                # what the project is and how to start; points at docs/index.md
CLAUDE.md                # agent instructions and toolchain specifics; also a brief top-level directory map
```

`docs/index.md` is the ground truth for a project's docs — the map of what actually exists. The recommended tree under it depends on the repo type. Use the matching reference to bootstrap a new docs tree or to audit an existing one for drift. The reference is a baseline, not a prescription — do not impose it on a project that has diverged; report drift instead.

| Repo type | What it is | Reference |
| - | - | - |
| System code repo | A deployed system with architecture, deployment, and modules | `reference/blueprint/system.md` |
| Library code repo | Code published for other repos to depend on; the public API surface is the primary artifact | `reference/blueprint/library.md` |
| Noncode repo | Docs are the repo's primary output — proposals, decisions, research; no code to describe | `reference/blueprint/noncode.md` |

Not every project needs every file in its reference tree; add what applies.

## Templates: content criteria by doc type

### Design docs

| Doc type | What it covers | Reference |
| - | - | - |
| System design doc | Module decomposition, public interface, data and control flow, data model and state, key algorithms, decisions, system e2e tests | `reference/templates/system_design.md` |
| Frontend design doc | Component tree, global state, data flow — for designs describing a frontend | `reference/templates/frontend_design.md` |
| UI design doc | Page structure and navigation, per-page interaction, chart visualization encoding | `reference/templates/ui_design.md` |

### Project docs

| Section | What it covers | Reference |
| - | - | - |
| Mission | Goal, problem statement, scope, stakeholders | `reference/templates/mission.md` |
| Usage scenarios | Concrete situations the project must handle | `reference/templates/usage_scenarios.md` |
| Roadmap | Vision, milestones, decisions log | `reference/templates/roadmap.md` |
| Tech stack | Languages, frameworks, toolchain, rationale | `reference/templates/tech_stack.md` |
| Concepts | Active domain vocabulary | `reference/templates/concepts.md` |
| API design (libraries) | Public API signatures, usage examples, design decisions | `reference/templates/api_design.md` |

### Other docs

| Doc type | What it covers | Reference |
| - | - | - |
| Research docs | Domain background, related-work comparison, design-option brainstorm | `reference/templates/research.md` |
| Dataset doc | Data acquisition, processing and description | `reference/templates/dataset.md` |

## Styles: markdown formatting

Opinionated doc formatting defaults — summary paragraph, numbered headings, no `---` separators, `-` bullets, key: value lists over two-column tables, Mermaid diagrams, `.markdownlint.json` — applied unless the project records different choices. Reference: `reference/styles/doc_style.md`.
````

- [ ] **Step 2: Retitle `reference/styles/doc_style.md`**

```markdown
# Documentation style (markdown)
```

→

```markdown
# Doc style (markdown)
```

Body stays verbatim.

- [ ] **Step 3: Verify**

Run: `grep -c "^---$" skills/aqr-doc-pattern/SKILL.md` — Expected: `2`
Run: `grep -rn "aqr-doc-blueprint\|aqr-doc-content\|aqr-style-rules" skills/aqr-doc-pattern/` — Expected: no output
Run: `head -1 skills/aqr-doc-pattern/reference/styles/doc_style.md` — Expected: `# Doc style (markdown)`

- [ ] **Step 4: Read `skills/aqr-doc-pattern/SKILL.md` end-to-end; confirm every table row points at a file that exists in `reference/`.**

- [ ] **Step 5: Commit**

```bash
git add skills/aqr-doc-pattern && git commit -m "skills: consolidate doc skills into aqr-doc-pattern"
```

---

### Task 4: Rewrite `templates/system_design.md` (slimmed criteria)

**Files:**
- Modify: `skills/aqr-doc-pattern/reference/templates/system_design.md` (full rewrite)

**Interfaces:**
- Produces: the "System e2e tests" section name that Task 3's index row and Task 9's CLAUDE.md already reference.

- [ ] **Step 1: Replace the file content with**

````markdown
# System design content criteria

Criteria for system design docs — design docs for system, layer, or module levels.

## 1. Purpose

A reviewer reads it and can reproduce the design's shape from the text, without reading code.

## 2. Content

Depth stops at component level. Each child is a black box; what is inside a component lives in code. If a unit follows a well-known pattern, name it and state only where this unit deviates.

- **Summary** — what this design covers and explicitly what it does not.
- **Public interface** — the upward contract this unit's parent defined for it, restated and shown as implemented. Shape it by unit type:
  - Website / frontend — the URL route map: each route and what it shows.
  - Network service — the endpoints: method, path, request/response fields, error shape.
  - Other module — its conceptual contract: what it receives, returns, and guarantees. No signature lists.
- **Decomposition and children** — if this unit splits into children, name each child module, its role and boundaries, its conceptual contract (what it receives, returns, guarantees), and how the children communicate and compose. Component is the deepest documented level.
- **Data model, state, and persistence** — the conceptual structures this unit owns; what state exists, who owns it, and how it persists. Conceptual level, not a full DDL. Omit if stateless.
- **Data flow and control flow** — how data enters, is transformed, is stored, and leaves; the request lifecycle, state transitions, concurrency, caching, failure and retry paths. Add a diagram when the flow is not obvious from prose, and describe it in prose first.
- **Key algorithms** — for each non-trivial part: a well-known algorithm or pattern → just name it; otherwise brief pseudocode; trivial → nothing. A choice among several strategies is a decision, not inline prose.
- **Key design and implementation decisions** — decision records, one per entry.
- **System e2e tests** — the end-to-end flows and scenarios to test for this system, and for each, conceptually how to test it. Unit-test design, test names, and method names live in code.

## 3. Constraints

- Explicit boundaries — every unit states what is inside and outside its responsibility.
- Conceptual only; implementation detail lives in code.
- No code inventories — no file lists; no class, method, or function names, whether of internals or of children; no test names. A child's contract is stated conceptually, not as a signature list.
- Be concise. Do not write code when a list of items will do — describe what and why, not how it is implemented.
````

- [ ] **Step 2: Verify**

Run: `grep -n "Testing approach\|signatures" skills/aqr-doc-pattern/reference/templates/system_design.md` — Expected: no output
Run: `grep -c "System e2e tests" skills/aqr-doc-pattern/reference/templates/system_design.md` — Expected: `1`

- [ ] **Step 3: Read the file end-to-end; confirm it matches the spec §7.1 section list.**

- [ ] **Step 4: Commit**

```bash
git add skills/aqr-doc-pattern/reference/templates/system_design.md && git commit -m "skills: slim system design criteria to component-level depth"
```

---

### Task 5: Rewrite `templates/frontend_design.md` (slimmed criteria)

**Files:**
- Modify: `skills/aqr-doc-pattern/reference/templates/frontend_design.md` (full rewrite)

**Interfaces:**
- Consumes: nothing from other tasks; its index row in Task 3 already matches.

- [ ] **Step 1: Replace the file content with**

````markdown
# Frontend design content criteria

Criteria for frontend design docs — when the design describes a frontend (a whole app or a single module).

## 1. Purpose

A reviewer reads it and understands the component architecture, state ownership, and data flow without reading component code.

## 2. Content

- **Summary** — what this design covers and explicitly what it does not.
- **Component tree** — the component hierarchy down to component level; a one-line role for each component; a reader can draw it from the text.
- **Global state store** — if the frontend uses a global store (e.g. Redux), describe the slices, what each holds, and who owns and who reads each slice.
- **Data flow and fetching architecture** — how data enters the frontend, where transforms happen (the fetch boundary), and how data reaches components. State how loading and error states surface, at the architecture level.
- **Other non-trivial design** — anything that deserves its own section: a new algorithm, cache design, optimistic-update strategy, etc.
- **Decisions** — decision records for significant design choices, one per entry.

## 3. Constraints

- The component is the leaf: no per-component state, data-fetching, or event-handler breakdowns.
- No hook, class, or method names — those live in code.
- Page- and flow-level behavior belongs in the UI design doc; unit-test design belongs in code.
````

- [ ] **Step 2: Verify**

Run: `grep -n "Per component\|Data fetching —\|Interaction —" skills/aqr-doc-pattern/reference/templates/frontend_design.md` — Expected: no output

- [ ] **Step 3: Read the file end-to-end; confirm it matches the spec §7.2 section list.**

- [ ] **Step 4: Commit**

```bash
git add skills/aqr-doc-pattern/reference/templates/frontend_design.md && git commit -m "skills: slim frontend design criteria to component-level depth"
```

---

### Task 6: Create aqr-presentation SKILL.md

**Files:**
- Create: `skills/aqr-presentation/SKILL.md`

**Interfaces:**
- Consumes: `skills/aqr-presentation/reference/presentation.md` (moved by Task 1, verbatim).
- Produces: skill name `aqr-presentation` (used by Task 9's CLAUDE.md).

- [ ] **Step 1: Write `skills/aqr-presentation/SKILL.md`**

````markdown
---
name: aqr-presentation
description: Opinionated PowerPoint (.pptx) style defaults — slide setup, text and bullets, content per slide, and render-and-inspect verification. Use when creating or editing a deck in a project that follows these defaults.
disable-model-invocation: false
---

# aqr-presentation

Opinionated defaults for `.pptx` decks — taste layered on top of the universal criteria, not a quality floor. Use the `pptx` skill for all pptx work; the reference below overrides or extends that skill's defaults. Apply unless the project records a different choice.

## Reference to look at

| Area | What it covers | Reference |
| - | - | - |
| Presentations (PowerPoint) | 16:10 layout, bullet symbols and margins, content per slide, render-and-inspect gate | `reference/presentation.md` |
````

- [ ] **Step 2: Verify**

Run: `grep -c "^---$" skills/aqr-presentation/SKILL.md` — Expected: `2`

- [ ] **Step 3: Commit**

```bash
git add skills/aqr-presentation && git commit -m "skills: add aqr-presentation for deck style defaults"
```

---

### Task 7: Correct aqr-cleanup scope (code projects only)

**Files:**
- Modify: `skills/aqr-cleanup/SKILL.md` (two exact edits)

**Interfaces:**
- Consumes: file moved by Task 1 (content unchanged so far).

- [ ] **Step 1: Edit the frontmatter description**

Old:

```markdown
description: Use when manually invoked to clean up an existing project. Thorough mode checks code/doc consistency, addresses code problems and quality risks, flags design-level complexity, and verifies fixes. Quick mode fixes only typos, stale references, and simple inconsistencies with no complexity analysis or refactoring.
```

New:

```markdown
description: Use when manually invoked to clean up an existing code project. Thorough mode checks code/doc consistency, addresses code problems and quality risks, flags design-level complexity, and verifies fixes. Quick mode fixes only typos, stale references, and simple inconsistencies with no complexity analysis or refactoring.
```

- [ ] **Step 2: Edit the body opening paragraph**

Old:

```markdown
Manual-only cleanup pass over an existing project. Do not auto-invoke; run only when explicitly named. The user specifies the mode in their request (e.g. "aqr-cleanup quick mode"). Default to thorough if unspecified.
```

New:

```markdown
Manual-only cleanup pass over an existing code project — a repo whose primary output is code, with docs that describe that code. A noncode repo (docs are the primary output) is out of scope: there is no code to clean, so review it with the normal doc criteria instead. Do not auto-invoke; run only when explicitly named. The user specifies the mode in their request (e.g. "aqr-cleanup quick mode"). Default to thorough if unspecified.
```

- [ ] **Step 3: Verify**

Run: `grep -n "code project" skills/aqr-cleanup/SKILL.md` — Expected: two lines (frontmatter description and body opening)
Run: `grep -n "existing project\." skills/aqr-cleanup/SKILL.md` — Expected: no output

- [ ] **Step 4: Commit**

```bash
git add skills/aqr-cleanup && git commit -m "skills: scope aqr-cleanup to code projects"
```

---

### Task 8: Update README.md

**Files:**
- Modify: `README.md` (intro paragraph, Skills section, References section; Agents and Layout sections unchanged)

**Interfaces:**
- Consumes: the five final skill names produced by Tasks 2, 3, 6, 7 and the unchanged `aqr-project-principle`.

- [ ] **Step 1: Replace the intro paragraph (line 3) with**

```markdown
Custom AI-coding skills and agents — two consolidated skills (code patterns, doc patterns), a presentation style skill, one cross-cutting working-principles skill, one manual-only cleanup skill, and one visual inspection subagent. Source repository — install by copying or symlinking the relevant directory into a project's `.claude/skills/` or `.claude/agents/` folder, then point the agent at it from that project's `CLAUDE.md`. Install per project only where wanted, not at user scope.
```

- [ ] **Step 2: Replace the entire `## Skills` section (through the line ending `invoked by name).`) with**

```markdown
## Skills

- `aqr-code-pattern` — Universal code quality principles plus opinionated frontend architecture patterns, design-system application guidance, and stack style defaults for code, tests, and notebooks.
- `aqr-doc-pattern` — The recommended docs layout (by repo type) plus content criteria for common documentation types and markdown style defaults.
- `aqr-presentation` — Opinionated PowerPoint style defaults for decks.
- `aqr-project-principle` — Working principles that define the quality bar for project work: what good work looks like, not a fixed workflow.
- `aqr-cleanup` — Manual-only cleanup pass over a code project: checks code/doc consistency, refactors code problems, verifies the refactor does not break behavior.

The four auto-invocable skills are doc only — no executable code. `aqr-cleanup` is manual-only (invoked by name).
```

- [ ] **Step 3: Replace the entire `## References` section (through end of file) with**

```markdown
## References

External sources adapted by these skills:

- [Documenting Architecture Decisions](https://cognitect.com/blog/2011/11/15/documenting-architecture-decisions) (M. Nygard, 2011) — the Architecture Decision Record format adapted in `aqr-doc-pattern`'s decision-record criteria.
- [architecture-decision-record](https://github.com/joelparkerhenderson/architecture-decision-record) (J. P. Henderson) — ADR templates and writing guidance.
- [Presentational and Container Components](https://medium.com/@dan_abramov/smart-and-dumb-components-7ca2f9a7c7d0) (D. Abramov) — the presentational/container distinction adapted in `aqr-code-pattern/reference/frontend_patterns.md` §1.3.
- [Software Architecture skill](https://github.com/davila7/claude-code-templates/blob/main/cli-tool/components/skills/development/software-architecture/SKILL.md) (claude-code-templates) — the library-first / NIH guidance and code-quality rules adapted in `aqr-code-pattern`.
```

- [ ] **Step 4: Verify**

Run: `grep -rn "aqr-code-criteria\|aqr-doc-blueprint\|aqr-doc-content\|aqr-style-rules\|aqr-design-system\|aqr-frontend-patterns\|reference/design.md" README.md` — Expected: no output

- [ ] **Step 5: Read README.md end-to-end.**

- [ ] **Step 6: Commit**

```bash
git add README.md && git commit -m "docs: update README to the consolidated skill set"
```

---

### Task 9: Update CLAUDE.md

**Files:**
- Modify: `CLAUDE.md` (six targeted section edits; other sections unchanged)

**Interfaces:**
- Consumes: all five final skill names (Tasks 2, 3, 6, 7 + `aqr-project-principle`).

- [ ] **Step 1: Replace the skills tree in the `## Layout` block with**

```
skills/
  <skill-name>/
    SKILL.md                  # skill definition: YAML frontmatter + body
    reference/                # explanatory docs loaded on demand; subfolders allowed
agents/<agent-name>.md        # subagent definition: YAML frontmatter + body
```

- [ ] **Step 2: Replace the entire `## Skills currently in this repo` section (from its heading up to but not including `## Agents currently in this repo`) with**

```markdown
## Skills currently in this repo

- `aqr-code-pattern` — Universal code quality and design principles plus opinionated frontend architecture patterns, design-system application guidance, and stack style defaults for code, tests, and notebooks. Applied when writing or reviewing code, UI, or design docs; criteria are not copied into the project.
- `aqr-doc-pattern` — The recommended docs layout by repo type plus content criteria for every standard doc type (system design, frontend design, UI design, project, research, dataset) and markdown formatting defaults. Applied when laying out, writing, or reviewing docs; criteria are not copied into the project.
- `aqr-presentation` — Opinionated PowerPoint style defaults for decks. Applied when creating or editing decks; not copied into the project.
- `aqr-project-principle` — Working principles that define the quality bar for project work: what good work looks like, not a fixed workflow. Applied during work; not copied into the project.
- `aqr-cleanup` — Manual-only cleanup pass over a code project. Thorough mode: checks code/doc consistency, addresses code problems and quality risks, flags design-level complexity, and verifies fixes. Quick mode: fixes only typos, stale references, and simple inconsistencies. Invoked by name only.

The four auto-invocable skills may be invoked by the host based on context, and the user can also name one directly via slash command. `aqr-cleanup` is manual-only.

The split is deliberate: code quality plus frontend architecture plus design-system application plus stack styles (`aqr-code-pattern`), docs layout plus content criteria plus doc formatting (`aqr-doc-pattern`), deck style (`aqr-presentation`), working principles (`aqr-project-principle`), and an on-demand cleanup pass (`aqr-cleanup`) are independent concerns. A project can adopt any combination.
```

- [ ] **Step 3: Replace item 3 of `## Editing skills` with**

```markdown
3. Cross-check the split: `aqr-code-pattern` covers code quality, frontend architecture, design-system application, and stack styles; `aqr-doc-pattern` covers docs layout, doc content, and doc formatting; `aqr-presentation` covers deck style only; `aqr-project-principle` covers working standards only; `aqr-cleanup` is an on-demand action, not a standing criterion. None of the standing criteria should overlap.
```

- [ ] **Step 4: In `## Installing skills and agents`, replace the first paragraph with**

```markdown
These are opt-in. Skills that state a universal floor are typically installed at user scope; taste-based skills at project scope, only in projects that want them. Copy or symlink each item verbatim into the host's location.
```

- [ ] **Step 5: Replace the two install path blocks' comments**

Claude Code block becomes:

```
~/.claude/skills/<skill-name>/               # user scope
<project>/.claude/skills/<skill-name>/        # project scope
<project>/.claude/agents/<agent-name>.md      # agents, project scope
```

opencode block becomes:

```
~/.config/opencode/skills/<skill-name>/       # user scope
<project>/.opencode/skills/<skill-name>/      # project scope
~/.config/opencode/agents/<agent-name>.md     # agents, user scope
<project>/.opencode/agents/<agent-name>.md    # agents, project scope
```

- [ ] **Step 6: Replace the adoption-directive example block (the fenced `md` block) with**

```md
For software-development work, use the AQR skills under `.claude/skills/` and invoke the one that matches the task (do not invoke all of them):

- `aqr-project-principle` — any non-trivial work (the quality bar for the work)
- `aqr-code-pattern` — writing or reviewing source code or design docs; frontend (React) code; UI against a design system; stack style defaults
- `aqr-doc-pattern` — laying out or auditing the `docs/` tree; writing or reviewing docs (system design, frontend design, UI design, project, research, dataset); markdown style
- `aqr-presentation` — creating or editing PowerPoint decks

`aqr-cleanup` and the agents under `.claude/agents/` are manual — invoke them by name when needed.
```

- [ ] **Step 7: Replace the frontmatter-glob line in `## Verifying changes` with**

```markdown
- `grep -n "^---$" skills/*/SKILL.md` — confirm every SKILL.md has YAML frontmatter (two `---` lines).
```

- [ ] **Step 8: Verify**

Run: `grep -rn "aqr-code-criteria\|aqr-design-system\|aqr-frontend-patterns\|aqr-doc-blueprint\|aqr-doc-content\|aqr-style-rules\|skills/general\|skills/opinionated\|skills/\*/\*/SKILL.md" CLAUDE.md` — Expected: no output

- [ ] **Step 9: Read CLAUDE.md end-to-end; confirm sections 1–7 above are applied and nothing else changed.**

- [ ] **Step 10: Commit**

```bash
git add CLAUDE.md && git commit -m "docs: update CLAUDE.md to the consolidated skill set"
```

---

### Task 10: Final verification sweep (spec §11)

**Files:**
- Fix: anything the sweep flags; otherwise no changes.

- [ ] **Step 1: Tree check**

Run: `find skills agents -type f | sort`
Expected: exactly the 23 files from Task 1's expected list, plus `skills/aqr-code-pattern/SKILL.md`, `skills/aqr-doc-pattern/SKILL.md`, `skills/aqr-presentation/SKILL.md` (26 files total).

- [ ] **Step 2: Stale-name sweep**

Run: `grep -rn "aqr-code-criteria\|aqr-design-system\|aqr-frontend-patterns\|aqr-doc-blueprint\|aqr-doc-content\|aqr-style-rules" skills agents CLAUDE.md README.md`
Expected: no output.

- [ ] **Step 3: Frontmatter and Status checks**

Run: `grep -ln "^---$" skills/*/SKILL.md agents/*.md && grep -c "^---$" skills/*/SKILL.md agents/*.md`
Expected: all six SKILL.md files plus `agents/visual.md`, each with exactly `2`.
Run: `grep -rn "^Status:" skills/ agents/`
Expected: no output.

- [ ] **Step 4: Cross-reference integrity**

Run: `grep -rhn "reference/" skills/*/SKILL.md | grep -o "reference[^ |]*" | sort -u` — every path listed must exist under the owning skill's `reference/`.

- [ ] **Step 5: Read every rewritten file end-to-end**

`skills/aqr-code-pattern/SKILL.md`, `skills/aqr-doc-pattern/SKILL.md`, `skills/aqr-presentation/SKILL.md`, `skills/aqr-doc-pattern/reference/templates/system_design.md`, `skills/aqr-doc-pattern/reference/templates/frontend_design.md`, `skills/aqr-doc-pattern/reference/styles/doc_style.md`, `skills/aqr-cleanup/SKILL.md`, `README.md`, `CLAUDE.md`.

- [ ] **Step 6: Fix anything flagged, commit if changes were made**

```bash
git add -A && git commit -m "skills: fix stragglers from consolidation sweep"
```

(If nothing was flagged, skip this commit — do not create an empty commit.)

---

## Self-Review

**1. Spec coverage:** §3 tree → Task 1; §5 aqr-code-pattern → Task 2; §6 aqr-doc-pattern → Task 3; §7.1 → Task 4; §7.2 → Task 5; §7.3 (no changes to ui_design/api_design) → guaranteed by verbatim moves in Task 1; §8 aqr-presentation → Task 6; §9 cleanup → Task 7; §10 README → Task 8, CLAUDE.md → Task 9; §11 verification → Task 10; §12 out-of-scope → no task touches those files. No gaps.

**2. Placeholder scan:** none — every write/replace step contains the full text; every verify step has an exact command and expected output.

**3. Consistency:** skill names (`aqr-code-pattern`, `aqr-doc-pattern`, `aqr-presentation`) are identical across Tasks 2, 3, 6, 7, 8, 9; reference paths (`reference/blueprint/…`, `reference/templates/…`, `reference/styles/…`) match the Task 1 move targets and the Task 3 index tables; the "System e2e tests" wording is identical in Tasks 3, 4, and 9.
