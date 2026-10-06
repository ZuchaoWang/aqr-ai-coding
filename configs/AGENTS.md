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
- Fall back to `zai-vision` if the subagent can't run.

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
