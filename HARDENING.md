<!-- markdownlint-disable -->

# Hardening Report: wadackel--files-sync-action/v4.0.2

> This file was generated automatically by the hardening agent.

**Policy SHA:** `d636be7e43ef829af6e853da6b3c7566db9f72fe`

**Test Policy SHA:** `843adf9e4b8f85d0c08b27b9d0b09dd094b54702`

**Harden Agent Version:** `2`

Action **wadackel--files-sync-action/v4.0.2** was hardened automatically. 3 finding(s) were identified and resolved across 1 iteration(s).

## Findings Fixed

### permissions (severity: medium)

missing-permissions: ci.yaml has no top-level permissions block, and the 'build_and_test' job has no job-level permissions block. Only the 'publish' job defines permissions. The 'build_and_test' job runs on pull_request events and inherits the default (broad) token permissions.

Locations:

- `.github/workflows/ci.yaml:14`

### permissions (severity: medium)

missing-permissions: commitlint.yaml has no top-level permissions block and the single 'commitlint' job has no job-level permissions block. The workflow runs on pull_request events and inherits the default (broad) token permissions.

Locations:

- `.github/workflows/commitlint.yaml:9`

### permissions (severity: medium)

missing-permissions: e2e.yaml has no top-level permissions block and the single 'test' job has no job-level permissions block. The workflow inherits the default (broad) token permissions.

Locations:

- `.github/workflows/e2e.yaml:9`

## Iteration Notes

### Iteration 1

**Fixes applied:** permissions

**Notes:**

Added explicit job-level permissions blocks to all three workflows:
- ci.yaml: Added `permissions: contents: read` to the `build_and_test` job. The `publish` job already had its own permissions block.
- commitlint.yaml: Added `permissions: contents: read` and `pull-requests: read` to the `commitlint` job, which needs to read repo contents and PR information.
- e2e.yaml: Added `permissions: contents: read` to the `test` job. The actual file sync work uses a GitHub App token (not the default GITHUB_TOKEN), so only read access is needed for the initial checkout step.

