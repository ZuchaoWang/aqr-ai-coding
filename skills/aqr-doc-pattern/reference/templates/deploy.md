# Deploy content criteria

## 1. Purpose

An operator reads it and can take a fresh build to a running deployment, verify it, and revert if needed.

## 2. Content

1. **Targets and configuration** — where the service runs; the configuration each environment needs. Secrets are referenced, never inlined.
2. **Build and deploy** — how a build is produced and shipped to each target, as ordered steps.
3. **Health and rollback** — how to verify a deployment works, and how to revert to the previous version.

## 3. Constraints

- Reference the pipeline and tooling by name rather than restating them.
- Keep secrets out of the doc; point at where they live.
