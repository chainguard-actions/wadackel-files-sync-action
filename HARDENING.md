<!-- markdownlint-disable -->

# Hardening Report: wadackel--files-sync-action/v4.0.1

> This file was generated automatically by the hardening agent.

**Policy SHA:** `d636be7e43ef829af6e853da6b3c7566db9f72fe`

**Test Policy SHA:** `843adf9e4b8f85d0c08b27b9d0b09dd094b54702`

**Harden Agent Version:** `2`

Action **wadackel--files-sync-action/v4.0.1** was hardened automatically. 3 finding(s) were identified and resolved across 1 iteration(s).

## Findings Fixed

### missing-permissions (severity: medium)

ci.yaml has no top-level `permissions:` key, and the `build_and_test` job has no job-level `permissions:` block. This means the job runs with default (potentially broad) GitHub token permissions. Only the `publish` job has explicit permissions defined.

Locations:

- `.github/workflows/ci.yaml:1`

### missing-permissions (severity: medium)

e2e.yaml has no top-level `permissions:` key and the `test` job has no job-level `permissions:` block. The job runs with default (potentially broad) GitHub token permissions.

Locations:

- `.github/workflows/e2e.yaml:1`

### missing-permissions (severity: medium)

commitlint.yaml has no top-level `permissions:` key and the `commitlint` job has no job-level `permissions:` block. The job runs with default (potentially broad) GitHub token permissions.

Locations:

- `.github/workflows/commitlint.yaml:1`

## Iteration Notes

### Iteration 1

**Fixes applied:** missing-permissions

**Notes:**

Added `permissions:` blocks to all three workflow files:

1. **ci.yaml**: Added top-level `permissions: contents: read` and job-level `permissions: contents: read` to the `build_and_test` job. The `publish` job already had explicit permissions (`contents: write`, `issues: write`, `pull-requests: write`) and was left unchanged.

2. **e2e.yaml**: Added top-level `permissions: contents: read` and job-level `permissions: contents: read` to the `test` job. The job uses a GitHub App token (not the default GITHUB_TOKEN) for the actual file sync operation, so no additional permissions are needed.

3. **commitlint.yaml**: Added top-level `permissions: contents: read` and job-level `permissions: contents: read` plus `pull-requests: read` to the `commitlint` job. The `pull-requests: read` permission is needed by the wagoid/commitlint-github-action to read PR commit information.

