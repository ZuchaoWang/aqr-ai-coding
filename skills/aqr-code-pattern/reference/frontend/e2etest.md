# Frontend e2e testing

Opinionated end-to-end testing pattern for frontend apps — Playwright is the example toolchain; the pattern applies to any comparable setup. The e2e suite runs the real frontend against a mock backend serving deterministic fixtures and drives it like a user. It comes in two forms: textual tests assert on the DOM/HTML, and visual tests compare screenshots of the live app against the design mockups — static HTML+CSS pages, one per canonical screen, kept in the repo as the design intent — to catch both drift from the design and unexpected breakage. Apply unless the project records a different choice; not copied into the project.

A canonical state is a URL plus the data and interaction state the design specifies (e.g. "orders page, table view, one order selected, animation paused"). The mockups each depict one canonical state; the tests reproduce the same states in the live app.

Example layout: `tests/e2e-textual/` (Playwright specs), `tests/e2e-visual/` (capture script + comparison prompt), `scripts/` (runner scripts), `design/` (static HTML mockups), `test-output/` (gitignored results).

## 1. Infrastructure

### 1.1 Mock backend with deterministic fixtures

- E2e never hits a real upstream: a lightweight server with the same API surface (FastAPI in the example) serves hardcoded fixture files — identical data on every run, so tests can assert exact table-row content and counts, and screenshots stay identical run-to-run so a pixel diff fails only when something actually changed.
- The dataset is canonical: tests read expected values straight from it (an entity's id, a filter value, a from/to pair) and export them as shared constants, instead of re-hardcoding them per spec.

### 1.2 Runner script: servers, ports, teardown

- The suite manages its own servers: a runner script starts the mock backend and the frontend dev server on free ports (never conflicting with a running dev environment), waits for both to accept TCP connections, runs the suite, and tears both down via an exit trap; the frontend port is passed to the test config.

### 1.3 Serial Playwright config

- Choose a fixed viewport that matches the design (1920×1080 for example) and use it for every test. Run the suite serially — one worker, no retries, no parallelism — so runs are deterministic and tests cannot interfere with each other.

## 2. Conventions the app must honor

### 2.1 Semantic `data-testid` on every major region

- Name by role and scope: `app-*` for app-global regions (header, tabs, view toggle), `<page>-<region>` for page regions (`dashboard-map`, `orders-table-view`). A region shared across pages keeps one id (`foo-map` appears on every foo page).
- The design mockup HTML (if available) carries the same testids — this shared vocabulary is what makes the geometry and visual comparisons possible.

### 2.2 ARIA contracts for interactive state

- Tests assert interactive state through the accessibility contract, never CSS classes: toggles are radios with accessible names (`getByRole('radio', {name})` plus `aria-checked`), and icon-only buttons expose state via `aria-label` (`Play`/`Pause`, `Expand`/`Collapse`).

### 2.3 URL as state

- SPA state is deep-linkable (hash routes + query params), so every test starts from `page.goto('/#/orders/table?customer=…')` and interaction tests assert URL sync after clicks.

### 2.4 Clean-console gate

- Every test fails on any `console.error` or uncaught `pageerror`: attach page listeners at test start, assert the collected list is empty at test end (one tick of flush first).

## 3. Textual tests — assert on the HTML/DOM

### 3.1 Page-load contract (existence of UI)

- One test per URL: deep-link in, wait for the major regions to settle, then assert which regions are in and out of the viewport with `toBeInViewport()` / `not.toBeInViewport()`. Several matchers look similar (`toBeVisible`, `toBeAttached`), but viewport containment is the contract: at a fixed design viewport, a visible-yet-offscreen region is a layout bug. Also assert selector values, the active radio, and row counts where the contract specifies. Shared header assertions live in one helper used by every case.

### 3.2 Interaction scenarios

- Click through tabs, toggles, and selectors; assert the URL synced and the right regions swapped in/out. Comboboxes are driven as a user would: `fill(...)` then `press('Enter')`.

### 3.3 Round-trip stability

- Capture an in-memory screenshot, navigate away and back, capture again, and compare with `pixelmatch` at a tiny diff tolerance (0.1%). No committed baseline — the comparison is purely in-session, so it fails only when state actually leaks across a navigation cycle.
- Before each capture, mask non-deterministic regions — hide the clock and animated canvases via inline `visibility:hidden` and pause animations. Masking serves this in-session pixel comparison only; the visual suite instead tells its comparison prompt to ignore those regions (§4.2).

### 3.4 Geometry comparison against the mockup (optional)

- When static HTML mockups exist: for each canonical state, open the mockup via `file://` and the live page at the same fixed viewport, collect `getBoundingClientRect()` of every `data-testid` element (rounded to 0.1px), and assert zero drift against the mockup.
- Per-state config carries the mockup file, the URL, a `setup` function to reproduce the state (expand a collapsed panel, switch view, pause animation), and an `exclude` list for elements that legitimately differ (e.g. controls the mockup renders always but the app hides at defaults).
- Write a JSON report per state; the failure message prints per-element deltas (`Δdx Δdy Δdw Δdh`) with both rects.

## 4. Visual tests — assert on screenshots

### 4.1 Capture mockup and live screenshots per state

- For each canonical state: screenshot the static mockup page and the live page in the same browser context (same viewport, `deviceScaleFactor: 1`), after `document.fonts.ready` and a settle wait, with `animations: 'disabled'`. Write `mockups/<state>.png`, `live/<state>.png`, and a `manifest.json` listing the pairs.
- The per-state `setup` reproduces the mockup's exact state in the live app — expand a collapsed panel, pause the animation, hover the marker that the mockup shows hovered (§5.1).

### 4.2 Comparison by a vision agent

- The comparison itself is delegated to a vision-capable agent run once per manifest entry. The prompt is predefined — written once, kept in the suite (e.g. `compare-prompt.md`), and sent unchanged for every state alongside the two screenshots and the project's design documentation (tokens, layout dimensions, component specs). It states, in order: the app context, what to compare (color palette, typography, spacing, layout, text content, chart rendering), what to ignore (anti-aliasing, non-deterministic regions such as a ticking clock, tooltip position, known mockup limitations), pass criteria (no drift a human would notice at arm's length), and failure reporting (element by `data-testid` or location, property, direction, severity).
- The unexpected-breakage clause matters most: flag any defect in the live screenshot that the mockup does not show — clipped or overflowing text, elements escaping their parent, unintended overlap, misalignment, raw unstyled HTML, stray scrollbars. These are real bugs, not acceptable drift.

## 5. Shared patterns

### 5.1 Canvas interaction by pixel color

Canvas charts (2D or WebGL) expose no DOM elements to select or hover, yet both suites must interact with them — textual tests hover a marker to assert its tooltip, and the visual capture hovers one to reproduce the mockup's state. The shared technique: drive the canvas by pixel color. In the example, the helper lives with the textual helpers and the visual capture imports it.

- Scan the canvas for the pixel nearest in RGB to the marker's fill color and move the mouse there; wrap in a retry (`expect(async () => {...}).toPass()`) so a cold, not-yet-painted canvas eventually passes.
- A color match can sit on a non-interactive overlay whose hover never engages the tooltip — exclude the area around each failed candidate and try the next. Exhaustion is a hard error, not a silent skip: a tooltip-less hover makes a textual assertion vacuous and produces a screenshot the visual comparison flags as drift.
