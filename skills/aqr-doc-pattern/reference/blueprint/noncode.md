# Noncode repo layout

A repo whose primary output is documentation, not code — design proposals, decision records, research notes, specs. Design docs read as proposals with alternatives and decisions; they may forward-reference where the design will eventually be implemented (often an external repo).

## 1. Recommended docs structure

`docs/index.md` is the ground truth for a repo's docs — the map of what actually exists. The shape below is a reference: use it to bootstrap a new docs tree or to audit an existing one for drift. It is not a prescription — do not impose it on a repo that has diverged; report drift instead.

```
docs/
  index.md                # documentation map: one section per top-level docs/ subdirectory

  project/
    mission.md            # what knowledge or decisions this repo captures; scope and audience
    roadmap.md            # open decisions and pending docs, in intended order

  research/
    background.md         # background knowledge for concepts and motivations
    related_works.md      # existing works related
    brainstorm.md         # discussion of possible directions

  design/                 # the repo's primary output: design proposals
    {{topic}}.md          # one design doc per topic
    {{topic}}/            # a topic with several sub-proposals
      index.md            #   topic overview: its sub-proposals as black boxes
      {{subtopic}}.md     #   one per sub-proposal
```

Not every noncode repo needs every file; add what applies.
