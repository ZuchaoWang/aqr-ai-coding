# Skills: consolidate into aqr-code-pattern and aqr-doc-pattern

# Design

## 1. Problem

The skills tree has grown scattered and miscategorized:

- `skills/general/` vs `skills/opinionated/` forces a classification the host should not need; the agent should choose which skill applies.
- Code-side skills are fragmented: `aqr-code-criteria`, `aqr-design-system`, `aqr-frontend-patterns`, plus style rules in `aqr-style-rules` — four files to load for one code task.
- Doc-side skills are fragmented: `aqr-doc-blueprint` (layout), `aqr-doc-content` (content criteria), plus markdown style in `aqr-style-rules/reference/documentation.md`.
- Some scopes are wrong: `aqr-cleanup` says "an existing project" but only makes sense for a code project; a docs/plan-only repo has no code to clean.
- `aqr-doc-content`'s system/frontend design criteria ask for too much depth — per-component state/fetching/interaction breakdowns and unit-test-level testing guidance, i.e. detail inside a component rather than the architecture of components.

## 2. Goal

Flatten the tree to `skills/<name>/`, consolidate into one code skill and one doc skill, and slim the design-doc criteria to component-level depth. Decisions already agreed with the user:

- Consolidated skills are named `aqr-code-pattern` and `aqr-doc-pattern`.
- `aqr-cleanup` stays a separate manual-only skill, scoped to code projects.
- `aqr-project-principle` stays a separate skill — it is the only cross-cutting one (applies to code, docs, and plan-only work).
- `aqr-style-rules` is absorbed: code/notebook styles into `aqr-code-pattern` (under `reference/styles/`), doc style into `aqr-doc-pattern` (under `reference/styles/`), presentation into a new `aqr-presentation` skill.
- References are organized into subfolders: patterns/design-system at `reference/` root for the code skill; `blueprint/` vs `templates/` for the doc skill.

## 3. Target tree

```
skills/
  aqr-code-pattern/            # consolidated code skill (auto-invocable)
    SKILL.md                   # universal code criteria + reference index
    reference/
      frontend_patterns.md     # from aqr-frontend-patterns (verbatim)
      design_system.md         # from aqr-design-system (verbatim)
      styles/                  # from aqr-style-rules
        code_python.md         #   (verbatim, renamed)
        code_javascript.md     #   (verbatim, renamed)
        notebooks.md           #   (verbatim)
  aqr-doc-pattern/             # consolidated doc skill (auto-invocable)
    SKILL.md                   # root entry points + index tables
    reference/
      blueprint/               # repo-type docs layouts (from aqr-doc-blueprint)
        system.md
        library.md
        noncode.md
      templates/               # per-doc-type content criteria (from aqr-doc-content)
        system_design.md       #   rewritten (slimmed)
        frontend_design.md     #   rewritten (slimmed)
        ui_design.md           #   verbatim
        api_design.md          #   verbatim
        mission.md             #   verbatim
        usage_scenarios.md     #   verbatim
        roadmap.md             #   verbatim
        tech_stack.md          #   verbatim
        concepts.md            #   verbatim
        research.md            #   verbatim
        dataset.md             #   verbatim
      styles/
        doc_style.md           # from aqr-style-rules/reference/documentation.md
  aqr-presentation/            # new (auto-invocable)
    SKILL.md
    reference/
      presentation.md          # from aqr-style-rules/reference/presentation.md (verbatim)
  aqr-cleanup/                 # manual-only, scope corrected to code projects
    SKILL.md
  aqr-project-principle/       # unchanged
    SKILL.md
agents/
  visual.md                    # unchanged
```

Deleted: `skills/general/`, `skills/opinionated/`, and the standalone skills `aqr-code-criteria`, `aqr-design-system`, `aqr-frontend-patterns`, `aqr-doc-blueprint`, `aqr-doc-content`, `aqr-style-rules`. Verbatim moves use `git mv` to preserve history.

## 4. Invocation policy

| Skill | Invocation | Scope |
| - | - | - |
| aqr-code-pattern | auto | code quality floor; frontend architecture; design-system application; stack styles |
| aqr-doc-pattern | auto | docs layout; doc content criteria; doc formatting style |
| aqr-presentation | auto | PowerPoint deck style |
| aqr-project-principle | auto | cross-cutting working principles |
| aqr-cleanup | manual only (`disable-model-invocation: true`) | on-demand cleanup of a code project |

Descriptions start with "Use when…" and state triggering conditions only, per skill-authoring convention.

## 5. aqr-code-pattern

- **Body** — the ten sections of `aqr-code-criteria` verbatim (decomposition, library-first, public surface, naming, error handling, comments, secrets, file hygiene, testing, before committing). Opening paragraph states the layering: universal criteria are the floor; the references below are stack-specific taste applied on top; nothing is copied into the project.
- **Patterns table** — routes to:
  - `reference/frontend_patterns.md` — React/Redux data flow, layering, and component structure. Load when writing or reviewing frontend code.
  - `reference/design_system.md` — applying an existing design system (build / extend / restyle / audit). Load when implementing, migrating, or auditing UI against a design system.
- **Styles table** — routes to `reference/styles/`: `code_python.md` (TypedDict/os.path choices, naming, ruff/pyright/pytest toolchain, project files, test shape), `code_javascript.md` (naming, .nvmrc, editorconfig), `notebooks.md` (kernel pin, required header, root-finding boilerplate). Opinionated defaults — apply unless the project records a different choice.
- **Frontmatter description** covers all four concerns (code criteria, frontend patterns, design-system work, stack styles) so the host auto-invokes for any of them.

## 6. aqr-doc-pattern

- **Body** — merges `aqr-doc-blueprint`'s SKILL.md (root entry points `README.md` + `CLAUDE.md`, the `docs/index.md` ground-truth rule, the repo-type table routing to `blueprint/system.md`, `blueprint/library.md`, `blueprint/noncode.md`) and `aqr-doc-content`'s SKILL.md (the doc-type tables routing to `templates/`). Opening paragraph states the three layers: blueprint = what docs exist and where; templates = what good content looks like; styles = formatting defaults.
- **Index tables** — same rows as today's doc-content tables, with two wording updates to match the slimmed criteria: system design row ends "…, decisions, system e2e tests"; frontend design row reads "Component tree, global state, data flow".
- **Styles row** — `styles/doc_style.md`: markdown formatting defaults (summary paragraph, numbered headings, no `---` separators, `-` bullets, key: value lists over two-column tables, Mermaid diagrams, `.markdownlint.json`), applied unless the project records different choices.
- **Frontmatter description** covers layout, content, and doc style.

## 7. Slimmed design-doc criteria

### 7.1 `templates/system_design.md` (rewritten)

Depth stops at component level. Each child is a black box; what is inside a component lives in code.

Content sections:

- **Summary** — what this design covers and explicitly what it does not.
- **Public interface** — the upward contract at the unit's own boundary: URL route map (website/frontend) or endpoints (method, path, request/response fields, error shape) for a network service. No signature lists for inner modules.
- **Decomposition and children** — each child as a black box: name, role, boundaries, conceptual contract (what it receives, returns, guarantees), and how children communicate and compose. Component is the deepest documented level.
- **Data model, state, and persistence** — conceptual structures the unit owns; omit if stateless.
- **Data flow and control flow** — how data enters, is transformed, stored, leaves; request lifecycle, state transitions, concurrency, caching, failure and retry paths; diagram when non-obvious, described in prose first.
- **Key algorithms** — well-known pattern → name it; otherwise brief pseudocode; trivial → nothing. A choice among strategies is a decision, not prose.
- **Key design and implementation decisions** — decision records, one per entry.
- **System e2e tests** (replaces "Testing approach") — a list of which end-to-end flows/scenarios to test for the system and, for each, conceptually how to test it. No unit-test design, no test or method names.

Constraints:

- Explicit boundaries — every unit states what is inside and outside its responsibility.
- Conceptual only; implementation detail lives in code.
- No code inventories — no file lists; no class, method, or function names, whether of internals or of children; no test names. A child's contract is stated conceptually, not as a signature list.
- Concise — describe what and why, not how.

### 7.2 `templates/frontend_design.md` (rewritten)

The per-component "State / Data fetching / Interaction" breakdown is dropped — that is inside-component detail.

Content sections:

- **Summary** — what this design covers and explicitly what it does not.
- **Component tree** — the hierarchy down to component level; one-line role per component; a reader can draw it from the text.
- **Global state store** — if used: the slices, what each holds, and who owns and who reads each slice.
- **Data flow and fetching architecture** — how data enters the frontend, where transforms happen (the fetch boundary), and how data reaches components; how loading and error states surface, at the architecture level.
- **Other non-trivial design** — anything that deserves its own section: a new algorithm, cache design, optimistic-update strategy.
- **Decisions** — decision records for significant choices, one per entry.

Constraints:

- Component is the leaf: no per-component state, data-fetching, or event-handler breakdowns.
- No hook, class, or method names — those live in code.
- Page- and flow-level behavior belongs in the UI design doc; unit-test design belongs in code.

### 7.3 Not slimmed

`ui_design.md` (pages, navigation, per-page interactions) and `api_design.md` (a library's public API signatures) keep their current depth — pages-and-flows and the public API surface are those docs' artifacts, not internal detail. `templates/` files other than the two above are moved verbatim.

## 8. aqr-presentation (new)

- `SKILL.md` — auto-invocable; description "Use when creating or editing PowerPoint (.pptx) decks…". Body: one overview paragraph (opinionated `.pptx` defaults; use the `pptx` skill for all pptx work; the rules override or extend that skill's defaults) plus a pointer to `reference/presentation.md`.
- `reference/presentation.md` — verbatim from `aqr-style-rules/reference/presentation.md` (16:10 setup, text and bullets, content per slide, render-and-inspect gate).

## 9. aqr-cleanup scope correction

- Description: "Use when manually invoked to clean up an existing **code project**…"
- Body: opening line states the skill applies to code projects only; a repo whose primary output is documentation (noncode repo) is out of scope — there is no code to clean; review it with the normal doc criteria instead. Within a code project, docs that describe the code remain in scope (code/doc inconsistencies are a core cleanup category).
- Mode mechanics (thorough / quick / report) unchanged.

## 10. README.md, CLAUDE.md, and cross-reference updates

### 10.1 README.md

- Intro and Skills section rewritten to the five-skill list, without the universal/opinionated grouping: `aqr-code-pattern`, `aqr-doc-pattern`, `aqr-presentation`, `aqr-project-principle`, `aqr-cleanup` (manual-only).
- The References section's bullets point at a `reference/design.md` that no longer exists. Repoint each adapted-technique bullet to its current home: ADR format → `aqr-doc-pattern` decision-record criteria; presentational/container → `aqr-code-pattern/reference/frontend_patterns.md`; library-first / code-quality rules → `aqr-code-pattern`.

### 10.2 CLAUDE.md

- Layout block: `skills/<skill-name>/SKILL.md` with optional `reference/` (subfolders allowed); agents unchanged.
- Skills list: five skills with one-line descriptions.
- Split rationale paragraph: `aqr-code-pattern` owns code quality, frontend architecture, design-system application, and stack styles; `aqr-doc-pattern` owns docs layout, content criteria, and doc formatting; `aqr-presentation` owns deck style; `aqr-project-principle` is cross-cutting; `aqr-cleanup` is an on-demand manual action.
- Editing-skills cross-check (item 3) updated to the new boundaries.
- Installing section: drop the general/opinionated classification; keep "copy or symlink verbatim", scope per skill chosen by the adopter (universal floor → user scope, taste → project scope).
- Adoption-directive example rewritten with the new names and triggers.
- Verification commands: the frontmatter glob changes from `skills/*/*/SKILL.md` to `skills/*/SKILL.md`.

## 11. Verification (inspection-based, per repo CLAUDE.md)

- `find skills agents -type f | sort` — tree matches section 3 exactly.
- `grep -rn "aqr-code-criteria\|aqr-design-system\|aqr-frontend-patterns\|aqr-doc-blueprint\|aqr-doc-content\|aqr-style-rules" skills agents CLAUDE.md README.md` — no stale names.
- `grep -n "^---$" skills/*/SKILL.md agents/*.md` — every SKILL.md and agent file has two `---` frontmatter lines.
- `grep -rn "^Status:" skills/ agents/` — nothing.
- Read every rewritten file end-to-end (`aqr-code-pattern/SKILL.md`, `aqr-doc-pattern/SKILL.md`, `aqr-presentation/SKILL.md`, `templates/system_design.md`, `templates/frontend_design.md`, `styles/doc_style.md`, `aqr-cleanup/SKILL.md`, README.md, CLAUDE.md).

## 12. Out of scope

- No content changes to the universal code criteria, frontend patterns, design-system, blueprint, or template files beyond the moves and the two rewrites in section 7.
- `agents/visual.md` untouched.
