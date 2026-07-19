# System code repo layout

A deployed system: code that runs, with architecture, deployment, and runtime. Modules compose into layers, and layers into the system.

## Recommended docs structure

`docs/index.md` is the ground truth for a project's docs — the map of what actually exists. The shape below is a reference: use it to bootstrap a new docs tree or to audit an existing one for drift. It is not a prescription — do not impose it on a project that has diverged; report drift instead.

```
docs/
  index.md                # documentation map: one section per top-level docs/ subdirectory

  project/
    mission.md            # what the project is for; problem statement and scope
    usage_scenarios.md    # concrete user-facing scenarios the project must support
    roadmap.md            # the sequence of development objectives: order, target dates, owners
    concepts.md           # active domain vocabulary: concepts the project uses, with definitions

  client_docs/            # verbatim requirements and feedback from the client
    {{date}}/             # snapshot of client materials received on that date

  migration/              # notes for migrating from a prior project
    {{old_project_name}}.md  # what carried over and what changed from the prior project

  architecture/
    design.md             # system-level design: public interface (incl. external API), components, data flow, key decisions
    deploy.md             # deployment topology, runtime environment, ops notes
    tech_stack.md         # languages, frameworks, libraries, and rationale

  uidesign/               # project-level UI design; omit for non-frontend projects
    {{feature}}/          # one feature per folder, can contain markdown, images, and other assets

  research/
    background.md         # background knowledge for concepts and motivations
    related_works.md      # existing works related
    brainstorm.md         # discussion of possible designs

  implementation/
    # One design doc per module, named after the module. The rule recurses at every
    # level: a unit (the implementation root, a layer, or a module) that is a single
    # module is one file; a unit with several modules is a folder.

    # Multi-stack project — one entry per stack (e.g. frontend, backend):
    {{layer}}.md              # a layer that is a single module
    {{layer}}/                # a layer with several modules
      index.md                #   layer design: its modules as black boxes
      modules/{{module}}.md   #   one per module; recurses identically

    # Single-stack project — same rule, no layer level:
    {{module}}.md             # the implementation, if it is a single module
    index.md                  # the implementation, if it has several modules
    modules/{{module}}.md     #   one per module; recurses identically

  data/
    {{dataset}}.md        # one doc per dataset
```

Not every project needs every file; add what applies.
