# Frontend architecture

Opinionated frontend architecture — design taste, not a universal quality floor. React / Redux Toolkit is the example stack; translate to any comparable frontend stack. Scope is architecture and layering only; formatting, naming, and universal code quality are out of scope. Apply unless the project records a different choice; not copied into the project.

## 1. Data flow and layering

Shared state belongs in a global store, data transforms at the fetch boundary, and data ownership in containers - not scattered across components.

### 1.1 Shared state lives in a global store, not component state

Use a global store for broadly-shared or persistent state - auth, theme, session, cached data - read directly by the components that need it, never drilled down through intermediaries as props. Locally-shared state that only nearby components need lives at their nearest common ancestor; drilling it through layers that don't use it signals it is owned too high. Keep component-local state for ephemeral, single-component UI only - selector input text, dropdown open/close, a transient UI flag. If it must survive navigation/reload, it belongs in the store. In React, Redux Toolkit (+ RTK Query for server state) is a common choice, with one slice per domain; `useState` is for ephemeral UI only.

When using RTK Query, prefer `currentData` over `data` for consistent data: `currentData` is `undefined` when the current arguments are `skipToken`, or have no cached entry yet. Using `data` instead leaks the last cached result for a different argument set.

### 1.2 Put data transformation in the data-fetching layer, not the component

The component should use fetched data directly as much as possible. Do transformation - sorting, reformatting, filtering - in the data-fetching layer so the component receives display-ready data and the transform runs once per fetch instead of every render. The only derivations that stay in the component are those that cross multiple data sources - never single-endpoint transformation. In React, the data-fetching layer is usually RTK Query, and the transform goes in `transformResponse`.

### 1.3 Containers own data and layout; presentational components stay pure

Page-level components are containers: they read the store, call the data-fetching layer, pass props down, and define the layout. Reusable components are presentational: props in, events out, no fetching or dispatching. A presentational component does not define its own absolute position; that is set by its parent's layout - it only sizes and lays out its own children. Mixing the two is a smell - pull fetching/state up into a container and keep the presentational UI pure. Extract a component when the same UI is needed a second time; also split a component that mixes the two roles.

## 2. Component structure

A component should have a consistent internal structure - ordered sections and extracted controllers only when complex.

### 2.1 Internal ordering

Keep component code in this reading order:

1. props and external dependencies - `useSelector`, `useDispatch`, `useContext`, router hooks
2. data sources and local state - RTK Query hooks (`useListRoutesQuery`), `useState`
3. derived state - `useMemo`, `useCallback`
4. effects / side-effects - `useEffect`, `useLayoutEffect`
5. event handlers - named functions wired to `onClick` / `onChange`
6. early returns (loading / error / empty)
7. main template / JSX

Do not place substantial business logic inside the template.

Mark each section boundary with a comment header (e.g. `// --- event handlers ---`); skip the comment for sections that are empty or a single line.

### 2.2 Extract complex controllers, leave simple ones inline

Extract a component's orchestration (data fetching, state, effects) into a custom hook (e.g. `useRouteTableData`) when it is independent and complex enough that the split makes the component clearer. Otherwise keep the logic inline with the §2.1 sectioned ordering and named handlers.

## 3. Routing and relative paths

The same build should be mountable at any URL prefix without a rebuild.

- Use a hash router (`/#/path?query`), not the history API: the server only ever sees `/`, so no SPA rewrite rules are needed and deep links keep working under any mount point. In React, `HashRouter`.
- Reference static assets with relative paths, never a leading `/`, so the bundle resolves under any base path; in Vite, set `base: './'`.
- Call the API with a relative base too (`fetch('api/...')`, no leading `/`): with a hash router the document path never changes, so calls resolve under the mount prefix (`/foo/bar/api/...`). Each mount then owns its API via proxy routing — two apps at `/foo/bar` and `/goo/baz` each reach their own backend without coordinating the origin root.
- The server serves the app at a trailing-slash URL (`/foo/bar/`) and redirects `/foo/bar` to it, so relative resolution is stable.
