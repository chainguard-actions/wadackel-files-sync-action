<!-- markdownlint-disable -->

# Hardening Report: wadackel--files-sync-action/v3.5.0

> This file was generated automatically by the hardening agent.

**Policy SHA:** `d636be7e43ef829af6e853da6b3c7566db9f72fe`

**Test Policy SHA:** `843adf9e4b8f85d0c08b27b9d0b09dd094b54702`

**Harden Agent Version:** `2`

Action **wadackel--files-sync-action/v3.5.0** was hardened automatically. 4 finding(s) were identified and resolved across 1 iteration(s).

## Findings Fixed

### unpinned-uses (severity: high)

Multiple workflow files and the composite action reference external actions using mutable version tags instead of pinned 40-character commit SHAs. This exposes the workflow to supply-chain attacks if a tag is moved or a repository is compromised.

Failing references:
- .github/workflows/ci.yaml: `actions/checkout@v4.1.7` (×2)
- .github/workflows/commitlint.yaml: `actions/checkout@v4.1.7`, `wagoid/commitlint-github-action@v6`
- .github/workflows/e2e.yaml: `actions/checkout@v4.1.7`, `actions/create-github-app-token@v1`
- .github/actions/setup-node/action.yaml: `pnpm/action-setup@v4.0.0`, `actions/setup-node@v4.0.4`

All should be pinned to full SHA digests, e.g. `actions/checkout@11bd71901bbe5b1630ceea73d27597364c9af683 # v4.1.7`.

Locations:

- `.github/workflows/ci.yaml:17`
- `.github/workflows/ci.yaml:46`
- `.github/workflows/commitlint.yaml:10`
- `.github/workflows/commitlint.yaml:13`
- `.github/workflows/e2e.yaml:9`
- `.github/workflows/e2e.yaml:14`
- `.github/actions/setup-node/action.yaml:4`
- `.github/actions/setup-node/action.yaml:6`

### missing-permissions (severity: medium)

ci.yaml has no top-level `permissions:` block and the `build_and_test` job has no job-level `permissions:` block. Only the `publish` job defines permissions. Without explicit permissions, the `build_and_test` job inherits the default repository token permissions (which may be write-all depending on repository settings), violating the principle of least privilege.

Locations:

- `.github/workflows/ci.yaml:1`

### missing-permissions (severity: medium)

commitlint.yaml has no top-level `permissions:` block and the single `commitlint` job has no job-level `permissions:` block. The workflow inherits default repository token permissions, which may be overly broad.

Locations:

- `.github/workflows/commitlint.yaml:1`

### missing-permissions (severity: medium)

e2e.yaml has no top-level `permissions:` block and the single `test` job has no job-level `permissions:` block. The workflow inherits default repository token permissions, which may be overly broad.

Locations:

- `.github/workflows/e2e.yaml:1`

## Iteration Notes

### Iteration 1

**Fixes applied:** unpinned-uses, missing-permissions

**Notes:**

Fixed all 8 unpinned action references by pinning them to full 40-character commit SHAs (with tag comments for readability): actions/checkout@v4.1.7→692973e3d937129bcbf40652eb9f2f61becf3332, wagoid/commitlint-github-action@v6→b948419dd99f3fd78a6548d48f94e3df7f6bf3ed, actions/create-github-app-token@v1→d72941d797fd3113feb6b93fd0dec494b13a2547, pnpm/action-setup@v4.0.0→fe02b34f77f8bc703788d5817da081398fad5dd2, actions/setup-node@v4.0.4→0a44ba7841725637a19e28fa30b79a866c81b0a6. Added top-level `permissions: {}` to ci.yaml, commitlint.yaml, and e2e.yaml to deny all permissions by default, then added minimal job-level permissions: build_and_test gets `contents: read`, commitlint gets `contents: read` and `pull-requests: read`, e2e test gets `contents: read`. The publish job's existing write permissions were preserved as-is.

