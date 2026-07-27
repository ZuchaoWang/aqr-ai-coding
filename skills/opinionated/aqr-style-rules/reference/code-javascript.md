# Code and test style (JavaScript)

Opinionated JavaScript-stack style defaults. Apply these unless the project records a different choice.

## 1. Naming

- Files: `kebab-case.js` (`.ts`, `.jsx`, `.tsx`); `PascalCase` for component files.
- Functions and variables: `camelCase`.
- Private members: `_camelCase` (leading underscore by convention).
- Classes and components: `PascalCase`.

## 2. Frontend data flow and layering

Shared state belongs in a global store, data transforms at the fetch boundary, and data ownership in containers - not scattered across components.

### 2.1 Shared state lives in a global store, not component state

Use a global store for broadly-shared or persistent state - auth, theme, session, cached data - read directly by the components that need it, never drilled down through intermediaries as props. Locally-shared state that only nearby components need lives at their nearest common ancestor; drilling it through layers that don't use it signals it is owned too high. Keep component-local state for ephemeral, single-component UI only - selector input text, dropdown open/close, a transient UI flag. If it must survive navigation/reload, it belongs in the store. In React, Redux Toolkit (+ RTK Query for server state) is a common choice, with one slice per domain; `useState` is for ephemeral UI only.

When using RTK Query, prefer `currentData` over `data` for consistent data: `currentData` is `undefined` when the current arguments are `skipToken`, or have no cached entry yet. Using `data` instead leaks the last cached result for a different argument set.

### 2.2 Put data transformation in the data-fetching layer, not the component

The component should use fetched data directly as much as possible. Do transformation - sorting, reformatting, filtering - in the data-fetching layer so the component receives display-ready data and the transform runs once per fetch instead of every render. The only derivations that stay in the component are those that cross multiple data sources - never single-endpoint transformation. In React, the data-fetching layer is usually RTK Query, and the transform goes in `transformResponse`.

### 2.3 Containers own data and layout; presentational components stay pure

Page-level components are containers: they read the store, call the data-fetching layer, pass props down, and define the layout. Reusable components are presentational: props in, events out, no fetching or dispatching. A presentational component does not define its own absolute position; that is set by its parent's layout - it only sizes and lays out its own children. Mixing the two is a smell - pull fetching/state up into a container and keep the presentational UI pure. Extract a component when the same UI is needed a second time; also split a component that mixes the two roles.

## 3. Frontend component structure

A component should have a consistent internal structure - ordered sections and extracted controllers only when complex.

### 3.1 Internal ordering

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

### 3.2 Extract complex controllers, leave simple ones inline

Extract a component's orchestration (data fetching, state, effects) into a custom hook (e.g. `useRouteTableData`) when it is independent and complex enough that the split makes the component clearer. Otherwise keep the logic inline with the §3.1 sectioned ordering and named handlers.

## 4. Project files and editor config

- `.nvmrc` - pins the Node version (single line, no `v` prefix, e.g. `20.18.0`); read by version managers (nvm, fnm, etc.).
- `.editorconfig` for JavaScript (`[*.{js,ts,jsx,tsx}]`): `indent_style = space`, `indent_size = 2`, `end_of_line = lf`, `charset = utf-8`, `insert_final_newline = true`, `trim_trailing_whitespace = true`. Line length is governed by the formatter (e.g. Prettier `printWidth`), not `.editorconfig`.
