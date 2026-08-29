<!-- markdownlint-disable -->

# Hardening Report: wadackel--files-sync-action/v3.6.1

> This file was generated automatically by the hardening agent.

**Policy SHA:** `d636be7e43ef829af6e853da6b3c7566db9f72fe`

**Test Policy SHA:** `843adf9e4b8f85d0c08b27b9d0b09dd094b54702`

**Harden Agent Version:** `2`

Action **wadackel--files-sync-action/v3.6.1** was hardened automatically. 3 finding(s) were identified and resolved across 1 iteration(s).

## Findings Fixed

### missing-permissions (severity: medium)

The workflow file has no top-level `permissions:` key and at least one job also has no `permissions:` key. In ci.yaml, the `build_and_test` job has no `permissions:` block (only the `publish` job does). Without explicit permissions, jobs inherit the default repository token permissions, which may be overly broad.

Locations:

- `.github/workflows/ci.yaml:1`

### missing-permissions (severity: medium)

The workflow file has no top-level `permissions:` key and the `commitlint` job has no job-level `permissions:` key. Without explicit permissions, the job inherits the default repository token permissions, which may be overly broad.

Locations:

- `.github/workflows/commitlint.yaml:1`

### missing-permissions (severity: medium)

The workflow file has no top-level `permissions:` key and the `test` job has no job-level `permissions:` key. Without explicit permissions, the job inherits the default repository token permissions, which may be overly broad.

Locations:

- `.github/workflows/e2e.yaml:1`

## Iteration Notes

### Iteration 1

**Fixes applied:** missing-permissions

**Notes:**

Added explicit permissions blocks to three workflow jobs: (1) ci.yaml `build_and_test` job: `permissions: {}` since it only runs build/test/lint/typecheck steps and requires no token access; (2) commitlint.yaml `commitlint` job: `permissions: contents: read` for repository checkout; (3) e2e.yaml `test` job: `permissions: contents: read` for repository checkout (the file sync operation uses a GitHub App token, not the workflow token). The `publish` job in ci.yaml already had appropriate permissions and was not modified.

