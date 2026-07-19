# Library code repo layout

A library or SDK published for other repos to depend on. Distribution is publish, not deploy. The public API surface — what consumers depend on — is the primary artifact.

## Recommended docs structure

`docs/index.md` is the ground truth for a project's docs — the map of what actually exists. The shape below is a reference: use it to bootstrap a new docs tree or to audit an existing one for drift. It is not a prescription — do not impose it on a project that has diverged; report drift instead.

```
docs/
  index.md                # documentation map: one section per top-level docs/ subdirectory

  project/
    mission.md            # what the library does, who depends on it, what it replaces
    roadmap.md            # the sequence of development objectives: order, target dates, owners
    concepts.md           # active vocabulary: concepts and API terms the library exposes
    api.md                # public API: reference and canonical usage examples
    api_design.md         # public API design: why the surface is shaped this way, stability and versioning decisions

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
```

Not every library needs every file; add what applies.
