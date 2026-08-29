<!-- markdownlint-disable -->

# Hardening Report: wadackel--files-sync-action/v4.0.0

> This file was generated automatically by the hardening agent.

**Policy SHA:** `d636be7e43ef829af6e853da6b3c7566db9f72fe`

**Test Policy SHA:** `843adf9e4b8f85d0c08b27b9d0b09dd094b54702`

**Harden Agent Version:** `2`

Action **wadackel--files-sync-action/v4.0.0** was hardened automatically. 3 finding(s) were identified and resolved across 1 iteration(s).

## Findings Fixed

### missing-permissions (severity: medium)

The workflow file has no top-level `permissions:` key and at least one job also has no `permissions:` key. In ci.yaml, the `build_and_test` job has no permissions block (only `publish` does). Without explicit permissions, the job inherits the default repository permissions, which may be overly broad.

Locations:

- `.github/workflows/ci.yaml:1`

### missing-permissions (severity: medium)

The workflow file has no top-level `permissions:` key and no job-level `permissions:` key on any job. The `test` job runs without any explicit permission restrictions, inheriting potentially broad default repository permissions.

Locations:

- `.github/workflows/e2e.yaml:1`

### missing-permissions (severity: medium)

The workflow file has no top-level `permissions:` key and no job-level `permissions:` key on any job. The `commitlint` job runs without any explicit permission restrictions, inheriting potentially broad default repository permissions.

Locations:

- `.github/workflows/commitlint.yaml:1`

## Iteration Notes

### Iteration 1

**Fixes applied:** missing-permissions

**Notes:**

Added explicit permissions blocks to all three workflow files:
- ci.yaml: Added `permissions: contents: read` to the `build_and_test` job (the `publish` job already had explicit permissions with contents/issues/pull-requests: write).
- e2e.yaml: Added `permissions: contents: read` to the `test` job.
- commitlint.yaml: Added `permissions: contents: read` and `pull-requests: read` to the `commitlint` job (pull-requests: read is appropriate since it runs on pull_request events and the commitlint action reads PR data).
All permissions follow the principle of least privilege.

