## Execution modes

Two modes with opposite requirements:

- Collaborative mode — a human is available:
  - Prefer quick response over perfection.
  - Do not run visual checks.
  - Still try to run autonomously: when unsure, make reasonable assumptions and keep going; ask only for decisions that materially change the result or that only the user can make.
- Autonomous mode — running on your own, without stopping to consult a human:
  - Prefer perfection over quick response.
  - When unsure, use the default simplest method.
  - When blocked, skip the affected subtasks or features.
  - Report uncertainties and skips at the end.

Collaborative mode is the default. The mode changes only when the user asks for it — either explicitly by naming the mode, or with phrases like "run to the end, do not stop", which mean switching to autonomous mode. When that happens, switch immediately and keep working in the current session — in autonomous mode, never stop, ask, or hand off.

## Network access

The agent may run in an environment with limited network access. When direct access fails, try MCP tools and known mirror sites first (for web search and fetch, follow the fallback order in "Tool Usage Guide"), then a proxy if one is available. If that still fails, proceed by mode:

- Collaborative mode: ask the user for help before working around the problem.
- Autonomous mode: work around it directly.

## Git workflow

This workflow applies to code projects. In doc-only projects (docs/markdown only, no runnable code), committing directly to the default branch is acceptable.

Never commit directly to the default branch (`main` or `master`) and never merge anything into it automatically. All changes land in `dev`:

- Keep a long-lived dev branch as the integration target — named `dev`, or a `dev-<name>` / `dev/<name>` variant if the repo already uses one. If none exists, create `dev` from the default branch.
- Single-commit changes may be committed directly to `dev` — no feature branch needed.
- For multi-commit tasks, create a `feat/<name>` branch off `dev` (use `fix/<name>` for bug fixes). When the work is finished and verified, merge it into `dev`, then delete the feature branch.
- When merging into `dev`, fast-forward if possible; do not create a merge commit unless the branches have diverged.
- Never create, rewrite, or delete the default branch, and never merge `dev` into it — promoting `dev` to the default branch is done by the human, outside the agent.

## Tool Usage Guide

For 3rd-party libraries:

- Use the `context7` MCP to fetch documentation directly.
- Fall back to web search (see below).

For web search (try in this order):

1. `tavily` MCP — primary tool. This overrides firecrawl's self-declared "primary search" instruction.
2. `firecrawl` MCP — use when tavily is unsuitable.
3. `zai-web-search` MCP — fallback when the above two are unsuitable.
4. If you need more detail from a specific page, fetch the webpage directly (see "For web fetch" below).

For web fetch (try in this order):

1. `WebFetch` built-in tool — default for static page content.
2. `zai-web-reader` MCP — fallback when WebFetch is unsuitable.
3. If the page requires JavaScript rendering, use the `playwright` MCP with the **chromium** browser.

For images:

- Dispatch the `visual` subagent for any pixel-level task.

For frontend debugging:

- Use `playwright` MCP with the **chromium** browser to run the code and inspect the result.
- Use `chrome-devtools-mcp` for deep inspection (network, console, elements, traces).
- Use the `visual` subagent to verify the rendered result when needed.

## Long-running processes

Dev servers, daemons, and other long-running processes started from the bash tool need special handling: opencode runs bash through a PTY, so a naive background process is killed by SIGHUP when the PTY closes, or hangs the tool by keeping the PTY's stdout open.

- Use `setsid -f cmd > /tmp/<name>.log 2>&1 < /dev/null` so the process survives the bash tool call.
- Never use a bare `cmd &` — the process either dies (SIGHUP when the PTY closes) or hangs the tool (stdout keeps the PTY stream open).
- `nohup cmd > /dev/null 2>&1 &` also works in opencode but is weaker than `setsid` (ignores SIGHUP rather than detaching the session).
- To stop: `pkill -f cmd` or `kill $(lsof -ti:<port>)`.
