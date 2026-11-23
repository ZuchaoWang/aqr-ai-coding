# Recommended third-party skills

Third-party skills worth installing for opencode. Section 1 covers installing
skills through an opencode plugin; the remaining sections list standalone
skills grouped by tool. Install each standalone skill by copying its folder
into `~/.config/opencode/skills/`.

## 1. plugins

Skills can also arrive as part of an opencode plugin, which bundles skills
with code that hooks into every session: add the plugin to the `plugins`
array in `~/.config/opencode/opencode.json`, and opencode fetches it on its
next start. Two plugins provide an agent-wide working process this way, and
they overlap, so install at most one of them; the default for this setup is
superpowers.

- `superpowers` — process skills for a coding agent: brainstorming before
  building, systematic debugging before fixing, test-driven development,
  writing and executing plans, and verifying work before claiming it is
  done. Install by adding
  `"superpowers@git+https://github.com/obra/superpowers.git"` to the
  `plugins` array.
- `opencode-planning-with-files` — file-based planning for a pure research
  agent: the plan lives in `task_plan.md`, `progress.md`, and `findings.md`,
  and the plugin injects it into every turn, reminds the agent after file
  writes, carries the plan through compaction, and adds the `pwf_init`,
  `pwf_status`, and `pwf_check` tools. Install by adding
  `"opencode-planning-with-files"` to the `plugins` array and copying its
  companion skill, `planning-with-files`, from the same repo into
  `~/.config/opencode/skills/`.

## 2. tavily

Install the CLI with `npm install -g @tavily/cli` and authenticate by setting
the `TAVILY_API_KEY` environment variable.

- `tavily-search` — search the web, extract page content, map site structure,
  and crawl sites through the `tvly` CLI. Use as the primary web search.
- `tavily-entity` — fill in a company, person, or organization record for a CRM
  from authoritative web sources through the `tvly` CLI.

## 3. firecrawl

Install the CLI with `npm install -g firecrawl-cli` and authenticate by setting
the `FIRECRAWL_API_KEY` environment variable.

- `firecrawl-search` — web search with query-relevant page excerpts and
  optional full-page content through the `firecrawl` CLI. Use as the fallback
  when tavily fails.

## 4. context7

Install the CLI with `npm install -g ctx7` (or run it ad hoc with
`npx ctx7@latest`) and authenticate by setting the `CONTEXT7_API_KEY`
environment variable.

- `find-docs` — retrieve up-to-date documentation, API references, and code
  examples for any library or framework through the `ctx7` CLI.

## 5. playwright

Install the CLI with `npm install -g @playwright/cli`; no authentication is
needed. The skill ships inside that npm package — copy its
`skills/playwright-cli/` folder into `~/.config/opencode/skills/`.

- `playwright-cli` — automate a browser through the `playwright-cli` CLI:
  navigate and interact with pages, take snapshots and screenshots, and inspect
  console output and network traffic.

## 6. chrome devtools

Install the CLI with `npm install -g chrome-devtools-mcp`; no authentication is
needed. The skill ships inside the same npm package — copy its
`skills/chrome-devtools-cli/` folder into `~/.config/opencode/skills/`, and
re-copy it whenever the CLI is updated because command syntax may change.

- `chrome-devtools-cli` — inspect a browser in depth through the
  `chrome-devtools` CLI: network activity, console output, performance traces,
  memory, and Lighthouse audits.

## 7. anthropic skills

No CLI is involved; install these skills from the `anthropics/skills` repo.

- `docx`, `pdf`, `pptx`, `xlsx` — create and edit Office documents.
- `skill-creator` — guidance for writing new skills.

## 8. K-Dense

Scientific-research skills from the `K-Dense-AI/scientific-agent-skills` repo.
There is no CLI to install: the skills run bundled Python
scripts against public research APIs, and the API keys they mention are
optional, read from the environment only to raise rate limits or unlock
optional features. The exception is `research-lookup`, whose default search
path requires a Parallel account (`PARALLEL_API_KEY`).

- `paper-lookup`, `research-lookup` — find scientific papers and compile
  current scholarly evidence for a research question.
- `literature-review` — conduct a comprehensive literature review.
- `scientific-critical-thinking`, `peer-review`, `scholar-evaluation` —
  evaluate scientific claims and evidence, review papers and manuscripts, and
  assess scholarly works.
- `hypothesis-generation`, `experimental-design` — develop testable research
  hypotheses and design rigorous experiments.
- `scientific-writing`, `citation-management` — write academic papers and
  manage references and citations.
- `statistical-analysis` — perform statistical analysis and interpret results.
- `scientific-slides`, `scientific-visualization`, `scientific-schematics` —
  create research presentations, publication-quality figures, and scientific
  diagrams.
- `markitdown` — convert PDFs and other documents into Markdown.

## 9. mermaid

No authentication is needed. The skill writes `.mmd` diagram text and exports
it to PNG, SVG, or PDF through the Kroki API over plain HTTP, or locally with
the `mmdc` CLI (`npm install -g @mermaid-js/mermaid-cli`). Install the
`mermaid-skill` skill from the `plugins/mermaid/skills/mermaid-skill` folder
of the `Agents365-ai/365-skills` repo; upstream marks it manual-only.

- `mermaid-skill` — create Mermaid diagrams with automatic layout:
  flowcharts, sequence and class diagrams, ER diagrams, state machines, and
  architecture overviews.
