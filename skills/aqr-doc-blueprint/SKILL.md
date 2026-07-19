---
name: aqr-doc-blueprint
description: Reference for the recommended docs layout — the `docs/` tree and the root entry points that route into it. Tells what docs a project should have and where; not how to write them. Use when laying out a project's docs, deciding where a doc belongs, or checking a docs tree for drift.
disable-model-invocation: false
---

# aqr-doc-blueprint

A recommended docs layout: what docs a project should have and where. It covers layout only — not how to write the content. Use it when creating or moving a doc to decide its name and location, and to see which docs a project needs.

## Root entry points

Two root files route readers and agents into the docs:

```
README.md                # what the project is and how to start; points at docs/index.md
CLAUDE.md                # agent instructions and toolchain specifics; also a brief top-level directory map
```

## Layout by repo type

`docs/index.md` is the ground truth for a project's docs — the map of what actually exists. The recommended tree under it depends on what kind of repo you are documenting. Use the matching reference to bootstrap a new docs tree or to audit an existing one for drift. The reference is a baseline, not a prescription — do not impose it on a project that has diverged; report drift instead.

| Repo type | What it is | Reference |
| - | - | - |
| System code repo | A deployed system with architecture, deployment, and modules | `reference/system.md` |
| Library code repo | Code published for other repos to depend on; the public API surface is the primary artifact | `reference/library.md` |
| Noncode repo | Docs are the repo's primary output — proposals, decisions, research; no code to describe | `reference/noncode.md` |

Not every project needs every file in its reference tree; add what applies.
