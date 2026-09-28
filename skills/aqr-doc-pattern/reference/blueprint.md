# Blueprint: docs tree

A reference structure for the `docs/` folder of code projects — deployed systems and libraries — with optional files marked. Not every project needs every file; add what applies.

## 1. Recommended docs structure

`docs/index.md` is the ground truth for a project's docs — the map of what actually exists. The shape below is a baseline: use it to bootstrap a new docs tree or to audit an existing one for drift. It is not a prescription — do not impose it on a project that has diverged; report drift instead.

```
docs/
  index.md                # documentation map: one section per top-level docs/ subdirectory

  project/
    mission.md            # what the project is for; problem statement, scope, stakeholders
    status.md             # current stage, next tasks, and project-level decisions made
    usage_scenarios.md    # concrete situations the project must handle, more detailed than mission
    concepts.md           # optional; active domain vocabulary, with definitions
    ui_design.md          # optional; only for projects with frontend interfaces; pages with url, conceptual behavior
    api_design.md         # optional; only for projects with public network/programming API: signatures and semantics

  research/               # optional; notes that inform decisions but are not specs
    background.md         # background knowledge for concepts and motivations
    related_works.md      # existing works related
    brainstorm.md         # discussion of possible designs

  implementation/         # the repo's implementation designs: system-level design plus one design doc per module
    design.md             # top-level implementation design: data and state, decomposition, data and control flow
    tech_stack.md         # languages, frameworks, toolchain, rationale
    deploy.md             # optional; only for projects which run as independent services

    modules/              # one implementation design doc per module, named after the module
      {{module_name}}.md  # that module's implementation design doc: data and state, decomposition, data and control flow, nontrivial implementation hints

  client_docs/            # optional; verbatim requirements and feedback from the client
    {{date}}/             # snapshot of client materials received on that date

  migration/              # optional; notes for migrating from a prior project
    {{old_project_name}}.md  # what carried over and what changed from the prior project

  data/                   # optional; one doc per dataset
    {{dataset}}.md
```
