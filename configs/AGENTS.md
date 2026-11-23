## Execution modes

Two modes with opposite requirements:

- Collaborative mode — a human is available:
  - Prefer quick response over perfection.
  - Do not run visual checks.
  - Ask the user when unsure.
- Autonomous mode — running on your own, without stopping to consult a human:
  - Prefer perfection over quick response.
  - When unsure, use the default simplest method.
  - When blocked, skip the affected subtasks or features.
  - Report uncertainties and skips at the end.

Collaborative mode is the default. The mode changes only when the user asks for it — either explicitly by naming the mode, or with phrases like "run to the end, do not stop", which mean switching to autonomous mode. When that happens, switch immediately and keep working in the current session — in autonomous mode, never stop, ask, or hand off.

## Task decomposition

Prefer splitting complex tasks into subtasks and delegating each to a subagent over doing everything in the main conversation.

- **Dispatching** — parallelize independent subtasks, at most 5 at a time; serialize only real dependencies. Give each subagent its own git worktree when running in parallel on the same repo (merge per "Git workflow"). Brief each fully — it starts with fresh context — and bound it: state the expected scope and a stop rule (a hung step or several calls without progress means stop and report partial status). Keep design decisions and final verification in the main conversation.
- **Reporting** — require each subagent to end its report with a status line: what was completed, what remains, files touched.
- **Checkpoints** — treat every `<subagent>` notification as a checkpoint: read the report and verify the assignment was actually completed; an error, or a truncated or missing report, means the subagent died. When you wake with an outstanding child that has sent no report, ping it by sessionID or re-dispatch if unreachable — before starting new work.
- **Recovery** — when a subagent dies or stalls, act immediately: follow up with its sessionID, re-dispatch a smaller version of the task, or absorb the remaining work into the main conversation, and tell the user what happened. Never acknowledge a failure and resume waiting; that freezes the whole task.
- **Waiting** — never idle by choice while children are outstanding: keep working your own remaining tasks and checkpoint silent children between chunks. When nothing is left to do, set at most one self-killing 30-minute watchdog against this session so a quietly hung child cannot freeze the task, say what is still running, and end the turn. Prefer the experimental automations API if the build has it; otherwise a detached `setsid` loop that posts a watchdog prompt every 30 minutes, tagged with the session id (e.g. `SUBAGENT-WATCHDOG-<sessionID>`) so parallel sessions never interfere; kill it once every child has reported.

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

For web search (try in this order):

1. Websearch-related skills — primary tool.
2. `zai-web-search` MCP — fallback when the skills are unsuitable.
3. If you need more detail from a specific page, fetch the webpage directly (see "For web fetch" below).

For web fetch (try in this order):

1. `WebFetch` built-in tool — default for static page content.
2. `zai-web-reader` MCP — fallback when WebFetch is unsuitable.
3. If the page requires JavaScript rendering, use the browser automation skill — **if installed**.

For images:

- Primary agents should dispatch the `visual` subagent for any pixel-level task; a subagent with native visual ability may do the work itself.

For frontend debugging:

- Use the browser automation skill — **if installed** — to run the code and inspect the result.
- Use the browser devtools skill — **if installed** — for deep inspection (network, console, elements, traces).
- Primary agents should use the `visual` subagent to verify the rendered result when needed; a subagent with native visual ability may verify it itself.

## Long-running processes

Use the `shell` tool's `background` option for dev servers, daemons, and other long-running processes. Background calls return immediately and notify the session when they finish; they have no timeout unless one is supplied.

- Set `background` instead of appending `&` — the harness manages the process and reports completion.
- Foreground calls time out after two minutes by default; pass a longer `timeout` (in milliseconds) when foreground work genuinely needs it.
- For a process that must outlive the OpenCode session itself, detach it: `setsid -f cmd > /tmp/<name>.log 2>&1 < /dev/null`.
- To stop a background process: `pkill -f cmd` or `kill $(lsof -ti:<port>)`.
