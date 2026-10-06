# Code and test style (JavaScript)

Opinionated JavaScript-stack style defaults. Apply these unless the project records a different choice.

## 1. Naming

- Files: `kebab-case.js` (`.ts`, `.jsx`, `.tsx`); `PascalCase` for component files.
- Functions and variables: `camelCase`.
- Private members: `_camelCase` (leading underscore by convention).
- Classes and components: `PascalCase`.

## 2. Project files and editor config

- `.nvmrc` - pins the Node version (single line, no `v` prefix, e.g. `24`); pin the Active LTS line, currently 24. Read by version managers (nvm, fnm, etc.).
- `.editorconfig` for JavaScript (`[*.{js,ts,jsx,tsx}]`): `indent_style = space`, `indent_size = 2`, `end_of_line = lf`, `charset = utf-8`, `insert_final_newline = true`, `trim_trailing_whitespace = true`. Line length is governed by the formatter (e.g. Biome line width), not `.editorconfig`.

## 3. Toolchain

- Node 24 — the Active LTS line, pinned via `.nvmrc`.
- TypeScript 7 (the native Go port), not 5.x.
- Biome for lint and format (config `biome.json` at the package root; run `biome check` / `biome check --write` on touched files). Chosen over ESLint+Prettier: TypeScript 7 has no stable programmatic API (until 7.1), which breaks `typescript-eslint`; Biome needs no TS API and does lint + format in one tool.
