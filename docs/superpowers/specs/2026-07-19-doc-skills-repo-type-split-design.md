# Doc skills: split by repo type

# Design

## 1. Problem

`aqr-doc-blueprint` and `aqr-doc-content` are written with a deployed system in mind: `architecture/deploy.md`, per-module `implementation/`, `client_docs/`, UI/frontend overlays, mission framed around end users. The layout and content criteria do not fit two other common repo shapes:

- **Library code repo** — published for others to depend on. There is no deployment, modules often collapse, and the public API surface (versioning, stability, examples) is the primary artifact.
- **Noncode repo** — docs are the output, not a description of code. There is no `implementation/`, no `architecture/deploy.md`. Design docs read as proposals with alternatives and decisions.

Today both skills force the system shape onto these repos, which produces drift or ignored docs.

## 2. Goal

Make both skills usable across three repo types with a small, predictable change: the SKILL.md becomes a short overview that routes by repo type to a per-type reference file. The current system-repo content is preserved (moved verbatim into `system.md` references); the library and noncode references are adapted from it.

## 3. Repo type taxonomy

Three buckets:

- **system** — a deployed system with modules. Has architecture, deployment, runtime. Current default.
- **library** — code published for other repos to depend on. Distribution is publish, not deploy. Public API surface is the primary artifact.
- **noncode** — docs are the repo's primary output. No code to describe; design docs are proposals with alternatives and decisions.

## 4. aqr-doc-blueprint changes

### 4.1 SKILL.md (overview)

Becomes a short overview:

- Scope statement (layout only, not content).
- "Root entry points" section (`README.md`, `CLAUDE.md`) — shared across repo types, stays inline.
- `docs/index.md` rule — shared, stays inline.
- Table routing each repo type to its reference file.

The detailed tree (current section "Recommended docs structure") moves out.

### 4.2 Reference files

- `reference/system.md` — the current "Recommended docs structure" verbatim (`project/`, `client_docs/`, `migration/`, `architecture/`, `research/`, `implementation/`, `data/`).
- `reference/library.md` — adapted:
  - `architecture/` keeps `design.md` and `tech_stack.md`; `deploy.md` is replaced by `publish.md` (packaging, distribution, versioning, deprecation).
  - `api/` is first-class: API reference and usage examples live here.
  - `implementation/` collapses (libraries usually have one or two layers).
  - `client_docs/` is renamed to `user_feedback/` (consumers, not clients).
  - `project/`, `research/`, `data/` unchanged.
- `reference/noncode.md` — adapted:
  - Drop `architecture/`, `implementation/`, `data/`, `migration/`.
  - Keep `project/` (charter, scope, audience).
  - Keep `research/`.
  - Add `design/` — the design proposals themselves, the repo's primary output.

## 5. aqr-doc-content changes

### 5.1 SKILL.md (overview)

Becomes a short overview:

- Scope statement.
- Table routing each (doc type × repo type) to its reference file.
- `research.md` and `dataset.md` stay shared across repo types — already generic.

### 5.2 Reference files

- `reference/design-system.md` — current `design.md` verbatim (universal contents, UI overlay, universal design criteria, UI criteria, decision recording).
- `reference/design-library.md` — adapted:
  - Public API surface as the central section: functions, types, traits; stability tags (stable / experimental / deprecated); semver implications of each.
  - Usage examples and integration patterns.
  - Versioning, backwards-compatibility, and deprecation policy.
  - De-emphasize internal module decomposition (it is the public surface that matters).
  - Keep the universal design criteria and decision-recording sections.
- `reference/design-noncode.md` — adapted:
  - Problem statement, goals and non-goals.
  - Proposed design (no reference to code in this repo — there is none).
  - Alternatives compared (this is the bulk).
  - Decisions and rationale.
  - Open questions.
  - Forward reference to where the design will be implemented (often an external repo or a follow-up system repo).
- `reference/project-system.md` — current `project.md` verbatim.
- `reference/project-library.md` — adapted:
  - Mission becomes "what the library does, who depends on it, what it replaces".
  - Usage scenarios become integration scenarios told from the consumer's perspective.
  - Publishing and distribution added (or referenced from `architecture/publish.md`).
  - Tech stack still applies.
  - Concepts becomes API vocabulary.
- `reference/project-noncode.md` — adapted:
  - Mission becomes "what knowledge or decisions this repo captures".
  - Scope = which topics or projects the repo covers.
  - Audience replaces users (who reads these docs and why).
  - No tech stack section.
  - Roadmap becomes "open decisions and pending docs".

### 5.3 Naming

Old `design.md` and `project.md` are renamed to `design-system.md` and `project-system.md` respectively — their content was already system-shaped. No generic version is kept.

## 6. Out of scope

- Splitting `research.md` or `dataset.md` — they already read generically.
- Adding a fourth bucket for "framework" repos (extensible libraries with plugin points). Recognized as a possible future addition.
- Touching the other AQR skills or agents.

## 7. Verification

By inspection, using the checks documented in the repo's `CLAUDE.md`:

- `find skills agents -type f | sort` — file tree intact, new reference files present, old names removed.
- `grep -rn "^Status:" skills/ agents/` — should return nothing.
- `grep -n "^---$" skills/*/SKILL.md` — every SKILL.md has YAML frontmatter.
- Read each new SKILL.md and reference file end-to-end.
