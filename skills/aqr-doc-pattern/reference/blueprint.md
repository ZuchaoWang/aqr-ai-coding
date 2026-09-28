# Blueprint: docs layout

A reference tree for code projects — deployed systems and libraries — with optional files marked. Noncode repos are an exception: docs are their primary output, the structure is simple, and it varies too much to prescribe. Keep a noncode repo's docs minimal — `index.md` plus folders as they arise — and do not impose this blueprint on them.

## 1. Root files

Two root files route readers and agents into the docs:

```
README.md                # what the project is and how to start; points at docs/index.md
CLAUDE.md                # agent instructions and toolchain specifics; also a brief top-level directory map
```

## 2. Recommended docs structure

`docs/index.md` is the ground truth for a project's docs — the map of what actually exists. The shape below is a reference: use it to bootstrap a new docs tree or to audit an existing one for drift. It is not a prescription — do not impose it on a project that has diverged; report drift instead. Files marked optional exist only when they apply; not every project needs every file — add what applies.

```
docs/
  index.md                # documentation map: one section per top-level docs/ subdirectory

  project/
    mission.md            # what the project is for; problem statement, scope, stakeholders
    roadmap.md            # development objectives: order, target dates, owners
    usage_scenarios.md    # optional; concrete situations the project must handle
    concepts.md           # optional; active domain vocabulary, with definitions
    ui_design.md          # optional; frontend projects; screens, flows, per-page behavior

  research/               # optional; notes that inform decisions but are not specs
    background.md         # background knowledge for concepts and motivations
    related_works.md      # existing works related
    brainstorm.md         # discussion of possible designs

  implementation/                 # the repo's designs: system-level design plus one design doc per module
    design.md             # top-level design: public interface (incl. external API), decomposition, data flow, key decisions
    deploy.md             # optional; deployment topology, runtime environment, ops notes
    tech_stack.md         # languages, frameworks, toolchain, rationale

    # Child design docs, by the recursion rule: a unit that is one design is one file;
    # a unit with several children is a folder with index.md (children as black boxes)
    # plus one design doc per child. The rule recurses at every level.
    {{child}}.md          # a unit that is a single module
    {{child}}/            # a unit with several children
      index.md            #   the unit's overview: its children as black boxes
      {{grandchild}}.md   #   one design doc per child; recurses identically

  client_docs/            # optional; verbatim requirements and feedback from the client
    {{date}}/             # snapshot of client materials received on that date

  migration/              # optional; notes for migrating from a prior project
    {{old_project_name}}.md  # what carried over and what changed from the prior project

  data/                   # optional; one doc per dataset
    {{dataset}}.md
```

For code repos the design children are modules, each doc named after its module. Libraries usually have one or two layers, so the recursion is shallow.
