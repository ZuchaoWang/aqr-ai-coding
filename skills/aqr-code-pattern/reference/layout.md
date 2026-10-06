# Project layout

Opinionated default repo layout for new projects — a default, not a mandate; apply unless the project records a different choice, and never copy this file into the project. Covers implementation files only — where code, tests, and scripts live. Documentation and other non-implementation directories (`docs/`, `data/`, `docker/`, editor config) are out of scope.

## 1. Frontend app: `frontend/` + `backend/`

A frontend project has exactly two top-level app directories plus `scripts/`. The frontend is the product; the backend exists to serve it — a mock server for development, a real server for production, either alone or both side by side behind the same API contract.

```
scripts/                   # repo-level setup / dev / build scripts

frontend/
  src/
    api/                  # API client and response types (the fetch boundary)
    pages/                # page containers, one folder per page
    components/           # page-specific components
    components_reusable/  # cross-page presentational components
    hooks/                # extracted controllers
    store/                # global store, one slice file per domain
    styles/
    utils/
  tests/
    unit/
    e2e-textual/
    e2e-visual/
  design/                 # design spec + HTML/CSS mockups (design reference)
  index.html, package.json, tsconfig.json, vite.config.ts, .nvmrc

backend/
  src/
    common/               # shared config, response envelope, middleware, models, logging
    mock/                 # mock server: deterministic fixtures
    real/                 # real server: serves real data
  tests/                  # mirrors src: mock/, real/
  scripts/                # backend-specific data download / extraction one-offs
  pyproject.toml, requirements.txt, requirements-dev.txt, .python-version
```

- `real` pairs with `mock` and is the default name; `live` is an alternative.
- Each backend server variant is a package with its own `app.py` entry point and `config.py`; domain modules inside it follow `views.py` (routes) + `services.py` (logic) per domain. Code shared between variants lives in `common/`, never duplicated.
- The frontend cannot tell which backend it is talking to — mock and real serve identical routes and response envelopes.
- Backend tests mirror `src/`: `tests/mock/`, `tests/real/`.

## 2. Pure backend app: top-level `src/` + `tests/`

A pure backend project is the backend shape flattened to the repo root — no wrapping `backend/` directory:

```
scripts/                   # setup / dev / build / data one-offs

src/
  <domain packages: views.py + services.py per domain, common/ for shared code>
tests/
pyproject.toml, requirements.txt, requirements-dev.txt, .python-version
```

## 3. Repo root

Only the directories above hold implementation files. Documentation and other non-implementation directories (`docs/`, `data/`, `docker/`, editor config) live at the repo root as needed but are out of scope here; do not invent further top-level code directories.
