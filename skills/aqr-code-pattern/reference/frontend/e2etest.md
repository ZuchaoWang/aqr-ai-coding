# Frontend e2e testing

Opinionated end-to-end testing pattern for frontend apps — Playwright is the example toolchain; the pattern applies to any comparable setup. The e2e suite runs the real frontend against a mock backend serving deterministic fixtures, drives it like a user, and comes in two forms: textual tests assert on the DOM/HTML, and visual tests compare screenshots against the design mockups to catch unexpected breakage. Apply unless the project records a different choice; not copied into the project.

Example layout: `tests/e2e-textual/` (Playwright specs), `tests/e2e-visual/` (capture script + comparison prompt), `scripts/` (runner scripts), `design/` (static HTML mockups), `test-output/` (gitignored results).

## 1. Infrastructure

### 1.1 Mock backend with deterministic fixtures

- E2e never hits a real upstream. A lightweight server with the same API surface (FastAPI in the example) serves hardcoded fixture files from a data directory — no auth complexity, no network flake, identical data on every run.
- The dataset is canonical: tests read expected values straight from it (a VP's asn/ip, a prefix, a src/dst pair) and export them as shared constants, instead of re-hardcoding them per spec.
- Deterministic data lets tests assert exact table-row content and counts, and keeps screenshots identical run-to-run so a pixel diff fails only when something actually changed.

### 1.2 Runner script: servers, ports, teardown

- One script per suite starts the mock backend and the frontend dev server on free ports, waits for TCP readiness, runs the suite, and tears both down via an exit trap. The frontend port is exported (e.g. `E2E_FRONTEND_PORT`) for the test config.

```bash
BACKEND_PORT=$(find_free_port 8000)
FRONTEND_PORT=$(find_free_port 5173)
export E2E_FRONTEND_PORT=$FRONTEND_PORT

cleanup() {
  for port in "$BACKEND_PORT" "$FRONTEND_PORT"; do
    pids=$(lsof -ti :"$port" 2>/dev/null || true)
    [ -n "$pids" ] && kill $pids 2>/dev/null || true
  done
}
trap cleanup EXIT INT TERM

scripts/dev-mock.sh "$BACKEND_PORT" > "$LOG_DIR/e2e-backend.log" 2>&1 &
scripts/dev-frontend.sh "$FRONTEND_PORT" "$BACKEND_PORT" > "$LOG_DIR/e2e-frontend.log" 2>&1 &
wait_for_port "$BACKEND_PORT" && wait_for_port "$FRONTEND_PORT"

cd frontend && npx playwright test --config tests/e2e-textual "$@"
```

### 1.3 Serial Playwright config

- Fixed viewport matching the design (1920×1080 in the example), one worker, no retries, no parallelism — deterministic and no cross-test interference.

```ts
export default defineConfig({
    testDir: '.',
    fullyParallel: false,
    workers: 1,
    retries: 0,
    use: {
        baseURL: `http://localhost:${process.env.E2E_FRONTEND_PORT || 5173}`,
        viewport: {width: 1920, height: 1080},
        actionTimeout: 10_000,
        navigationTimeout: 30_000,
    },
});
```

## 2. Conventions the app must honor

### 2.1 Semantic `data-testid` on every major region

- Name by role and scope: `app-*` for global chrome (header, tabs, view toggle), `<page>-<region>` for page regions (`monitor-map`, `route-table-view`). A region shared across pages keeps one id (`route-map` appears on every route page).
- The design mockup HTML carries the same testids — this shared vocabulary is what makes the geometry and visual comparisons possible.

### 2.2 ARIA contracts for interactive state

- Toggles render as radios with accessible names; tests assert the active one via `getByRole('radio', {name}).toHaveAttribute('aria-checked', 'true')`.
- Icon-only buttons expose state through `aria-label` (`Play`/`Pause`, `Expand`/`Collapse`), so tests assert state through the accessibility contract, never CSS classes.

### 2.3 URL as state

- SPA state is deep-linkable (hash routes + query params), so every test starts from `page.goto('/#/route/table?asn=…')` and interaction tests assert URL sync after clicks.

### 2.4 Clean-console gate

- Every test fails on any `console.error` or uncaught `pageerror`. Attach at test start; assert when the returned function is awaited at test end.

```ts
export function expectCleanConsole(page: Page) {
    const errors: string[] = [];
    page.on('console', msg => {
        if (msg.type() === 'error') errors.push(`console.error: ${msg.text()}`);
    });
    page.on('pageerror', err => errors.push(`pageerror: ${err.message}`));
    return async () => {
        await page.waitForTimeout(0); // flush pending console output
        expect(errors, errors.join('\n')).toEqual([]);
    };
}
```

## 3. Textual tests — assert on the HTML/DOM

### 3.1 Page-load contract (existence of UI)

- One test per URL: deep-link in, wait for the major regions to settle, then assert which regions are in and out of the viewport, selector values, the active radio, and row counts where the contract specifies. Shared header assertions live in one helper used by every case.

```ts
test('route/table — geo, VP selected', async ({page}) => {
    const clean = expectCleanConsole(page);
    await page.goto(`/#/route/table?${VP_QUERY}`);
    await expect(page.getByTestId('route-map')).toBeInViewport();
    await expect(page.getByTestId('route-table-view')).toBeInViewport();
    await expect(page.getByTestId('route-as-ring')).not.toBeInViewport();
    await expect(page.getByTestId('route-vp-select').locator('input')).toHaveValue(/AS3130/);
    await clean();
});
```

### 3.2 Interaction scenarios

- Click through tabs, toggles, and selectors; assert the URL synced and the right regions swapped in/out. Comboboxes are driven as a user would: `fill('AS3130 147.28.0.3')` then `press('Enter')`.

```ts
test('page tabs: monitor <-> route', async ({page}) => {
    const clean = expectCleanConsole(page);
    await page.goto('/#/monitor');
    await page.getByTestId('app-page-tabs').getByRole('radio', {name: '查询'}).click();
    await expect(page).toHaveURL(/\/route\/table/);
    await expect(page.getByTestId('route-page')).toBeInViewport();
    await expect(page.getByTestId('monitor-page')).not.toBeInViewport();
    await clean();
});
```

### 3.3 Round-trip stability

- Capture an in-memory screenshot, navigate away and back, capture again, and compare with `pixelmatch`. No committed baseline — the comparison is purely in-session, so it fails only when state actually leaks across a navigation cycle.
- Before each capture, hide non-deterministic regions (clock, animated canvases) via inline `visibility:hidden` and pause animations; the diff ratio tolerance is tiny (0.1%).

```ts
const mismatched = pixelmatch(a.data, b.data, diff.data, width, height, {threshold: 0.1});
expect(mismatched / (width * height)).toBeLessThanOrEqual(0.001);
```

### 3.4 Geometry comparison against the mockup (optional)

- When static HTML mockups exist: for each canonical state, open the mockup via `file://` and the live page at the same fixed viewport, collect `getBoundingClientRect()` of every `data-testid` element (rounded to 0.1px), and assert zero drift against the mockup.
- Per-state config carries the mockup file, the URL, a `setup` function to reproduce the state (open the fold, switch view, pause animation), and an `exclude` list for elements that legitimately differ (e.g. controls the mockup renders always but the app hides at defaults).
- Write a JSON report per state; the failure message prints per-element deltas (`Δdx Δdy Δdw Δdh`) with both rects.

```ts
async function collectRects(page: Page): Promise<Record<string, Rect>> {
    return page.evaluate(() => {
        const rects: Record<string, Rect> = {};
        document.querySelectorAll('[data-testid]').forEach(el => {
            const r = el.getBoundingClientRect();
            rects[el.getAttribute('data-testid')!] = {
                x: Math.round(r.x * 10) / 10,
                y: Math.round(r.y * 10) / 10,
                w: Math.round(r.width * 10) / 10,
                h: Math.round(r.height * 10) / 10,
            };
        });
        return rects;
    });
}
```

## 4. Visual tests — assert on screenshots

### 4.1 Capture mockup and live screenshots per state

- For each canonical state: screenshot the static mockup page and the live page in the same browser context (same viewport, `deviceScaleFactor: 1`), after `document.fonts.ready` and a settle wait, with `animations: 'disabled'`. Write `mockups/<state>.png`, `live/<state>.png`, and a `manifest.json` listing the pairs.
- The per-state `setup` reproduces the mockup's exact state in the live app — open the fold, pause the animation, hover the marker that the mockup shows hovered.

### 4.2 Canvas interaction by pixel color

- Canvas charts have no DOM targets, so hover a marker by scanning the canvas for its fill color and moving the mouse to the nearest match; wrap in a retry (`expect(async () => {...}).toPass()`) so a cold, not-yet-painted canvas eventually passes; treat candidate exhaustion (tooltip never engages) as a hard error, not a silent skip.

```ts
export async function hoverCanvasColor(
    page: Page, canvas: Locator,
    target: [number, number, number], timeout = 10_000,
) {
    await expect(async () => {
        const pt = await findCanvasPixel(canvas, target); // nearest-RGB scan
        await page.mouse.move(pt.x, pt.y);
    }).toPass({timeout});
}
```

### 4.3 Comparison by a vision agent

- The comparison itself is delegated to a vision-capable agent run once per manifest entry, with a structured prompt alongside both screenshots and the design doc. The prompt states, in order: the app context, what to compare (color palette, typography, spacing, layout, text content, chart rendering), what to ignore (anti-aliasing, masked clock, tooltip position, known mockup limitations), pass criteria (no drift a human would notice at arm's length), and failure reporting (element by `data-testid` or location, property, direction, severity).
- The unexpected-breakage clause matters most: flag any defect in the live screenshot that the mockup does not show — clipped or overflowing text, elements escaping their parent, unintended overlap, misalignment, raw unstyled HTML, stray scrollbars. These are real bugs, not acceptable drift.
- Non-deterministic regions (a ticking clock) are masked with a solid patch before capture, and the prompt says to ignore them.
