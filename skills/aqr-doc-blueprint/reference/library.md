# Library code repo layout

A library or SDK published for other repos to depend on. Distribution is publish, not deploy. The public API surface — what consumers depend on — is the primary artifact, so it gets first-class treatment.

## Root entry points

Two root files route readers and agents into the docs:

```
README.md                # what the library does, how to install it, a getting-started snippet; points at docs/index.md
CLAUDE.md                # agent instructions and toolchain specifics; also a brief top-level directory map
```

Top-level repo orientation — what each top-level directory is for — belongs here in CLAUDE.md/README, not in a separate layout doc. Stack-specific root files (version pins, manifests, editor and lint config) are per-stack conventions, outside this layout.

## Recommended docs structure

`docs/index.md` is the ground truth for a project's docs — the map of what actually exists. The shape below is a reference: use it to bootstrap a new docs tree or to audit an existing one for drift. It is not a prescription — do not impose it on a project that has diverged; report drift instead.

```
docs/
  index.md                # documentation map: one section per top-level docs/ subdirectory

  project/
    mission.md            # what the library does, who depends on it, what it replaces
    usage_scenarios.md    # integration scenarios, told from the consumer's perspective
    roadmap.md            # the sequence of development objectives: order, target dates, owners
    concepts.md           # active vocabulary: concepts and API terms the library exposes

  user_feedback/          # verbatim requests and feedback from consumers
    {{date}}/             # snapshot of materials received on that date

  architecture/
    design.md             # library-level design: public API surface, module decomposition, key decisions
    publish.md            # packaging, distribution channels, versioning and deprecation policy
    tech_stack.md         # languages, frameworks, libraries, and rationale

  api/                    # the public surface, first-class
    reference.md          # generated or hand-written API reference
    usage_examples.md     # canonical usage patterns and recipes

  research/
    background.md         # background knowledge for concepts and motivations
    related_works.md      # existing libraries this one relates to or replaces
    brainstorm.md         # discussion of possible designs

  implementation/
    # Libraries usually have one or two layers, so the recursion is shallow. The rule
    # is unchanged: a unit that is a single module is one file; a unit with several
    # modules is a folder.
    {{module}}.md             # the implementation, if it is a single module
    index.md                  # the implementation, if it has several modules
    modules/{{module}}.md     #   one per module; recurses identically

  data/
    {{dataset}}.md        # one doc per dataset
```

Not every library needs every file; add what applies.
