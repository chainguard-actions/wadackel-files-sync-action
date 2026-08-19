<!-- markdownlint-disable -->

# Hardening Report: wadackel--files-sync-action/v3.4.0

> This file was generated automatically by the hardening agent.

**Policy SHA:** `d636be7e43ef829af6e853da6b3c7566db9f72fe`

**Test Policy SHA:** `843adf9e4b8f85d0c08b27b9d0b09dd094b54702`

**Harden Agent Version:** `2`

Action **wadackel--files-sync-action/v3.4.0** was hardened automatically. 2 finding(s) were identified and resolved across 1 iteration(s).

## Findings Fixed

### unpinned-uses (severity: high)

Multiple workflow files reference external actions using mutable version tags instead of full 40-character commit SHAs. This exposes the workflow to supply-chain attacks if the tag is moved or the upstream repository is compromised. Failing references:
- `actions/checkout@v4.1.7` (tag, not SHA)
- `actions/create-github-app-token@v1` (tag, not SHA)
- `wagoid/commitlint-github-action@v6` (tag, not SHA)
- `pnpm/action-setup@v4.0.0` (tag, not SHA)
- `actions/setup-node@v4.0.3` (tag, not SHA)

Locations:

- `.github/workflows/ci.yaml:17`
- `.github/workflows/ci.yaml:50`
- `.github/workflows/e2e.yaml:10`
- `.github/workflows/e2e.yaml:14`
- `.github/workflows/commitlint.yaml:13`
- `.github/workflows/commitlint.yaml:17`
- `.github/actions/setup-node/action.yaml:5`
- `.github/actions/setup-node/action.yaml:7`

### missing-permissions (severity: medium)

Three workflow files have no top-level `permissions:` block and at least one job also lacks a `permissions:` block. Without explicit permissions, jobs inherit the default repository token permissions (which may be read/write for all scopes depending on repository settings), violating the principle of least privilege.

- `ci.yaml`: The `build_and_test` job has no `permissions:` block and there is no top-level `permissions:` key.
- `e2e.yaml`: No top-level `permissions:` and the `test` job has no `permissions:` block.
- `commitlint.yaml`: No top-level `permissions:` and the `commitlint` job has no `permissions:` block.

Locations:

- `.github/workflows/ci.yaml:1`
- `.github/workflows/e2e.yaml:1`
- `.github/workflows/commitlint.yaml:1`

## Iteration Notes

### Iteration 1

**Fixes applied:** unpinned-uses, missing-permissions

**Notes:**

Fixed all unpinned action references by resolving them to full 40-character commit SHAs using lookup_action_sha, preserving the original tag as a comment. Added top-level `permissions: {}` to ci.yaml, e2e.yaml, and commitlint.yaml to deny all permissions by default, and added job-level `permissions: contents: read` to the build_and_test, test, and commitlint jobs. The publish job's existing permissions (contents: write, issues: write, pull-requests: write) were preserved as they are needed for the release process.

