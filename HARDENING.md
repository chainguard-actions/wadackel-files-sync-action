<!-- markdownlint-disable -->

# Hardening Report: wadackel--files-sync-action/v3.6.0

> This file was generated automatically by the hardening agent.

**Policy SHA:** `d636be7e43ef829af6e853da6b3c7566db9f72fe`

**Test Policy SHA:** `843adf9e4b8f85d0c08b27b9d0b09dd094b54702`

**Harden Agent Version:** `2`

Action **wadackel--files-sync-action/v3.6.0** was hardened automatically. 3 finding(s) were identified and resolved across 1 iteration(s).

## Findings Fixed

### missing-permissions (severity: medium)

The workflow file has no top-level `permissions:` key and the `build_and_test` job has no job-level `permissions:` key. Only the `publish` job defines permissions. Without explicit permissions, GitHub grants the default (potentially broad) token permissions to the `build_and_test` job.

Locations:

- `.github/workflows/ci.yaml:1`

### missing-permissions (severity: medium)

The workflow file has no top-level `permissions:` key and the single `commitlint` job has no job-level `permissions:` key. Without explicit permissions, GitHub grants the default (potentially broad) token permissions to all jobs.

Locations:

- `.github/workflows/commitlint.yaml:1`

### missing-permissions (severity: medium)

The workflow file has no top-level `permissions:` key and the `test` job has no job-level `permissions:` key. Without explicit permissions, GitHub grants the default (potentially broad) token permissions to all jobs.

Locations:

- `.github/workflows/e2e.yaml:1`

## Iteration Notes

### Iteration 1

**Fixes applied:** missing-permissions

**Notes:**

Added explicit job-level permissions blocks to all three workflow files: (1) ci.yaml: added `permissions: contents: read` to the `build_and_test` job (the `publish` job already had its own permissions defined); (2) commitlint.yaml: added `permissions: contents: read` and `pull-requests: read` to the `commitlint` job; (3) e2e.yaml: added `permissions: contents: read` to the `test` job. All permissions follow the principle of least privilege.

