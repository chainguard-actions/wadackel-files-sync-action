<!-- markdownlint-disable -->

# Hardening Report: wadackel--files-sync-action/v3.3.0

> This file was generated automatically by the hardening agent.

**Policy SHA:** `d636be7e43ef829af6e853da6b3c7566db9f72fe`

**Test Policy SHA:** `843adf9e4b8f85d0c08b27b9d0b09dd094b54702`

**Harden Agent Version:** `2`

Action **wadackel--files-sync-action/v3.3.0** was hardened automatically. 4 finding(s) were identified and resolved across 1 iteration(s).

## Findings Fixed

### unpinned-uses (severity: high)

Multiple `uses:` references are pinned to version tags instead of full 40-character commit SHAs, making them vulnerable to supply-chain attacks if the tag is moved. Failing references:
- `actions/checkout@v4.1.7` (ci.yml, ×2)
- `actions/checkout@v4.1.7` (commitlint.yml)
- `wagoid/commitlint-github-action@v6` (commitlint.yml)
- `actions/checkout@v4.1.7` (e2e.yml)
- `actions/create-github-app-token@v1` (e2e.yml)
- `pnpm/action-setup@v4.0.0` (setup-node/action.yml)
- `actions/setup-node@v4.0.3` (setup-node/action.yml)

Locations:

- `.github/workflows/ci.yml:17`
- `.github/workflows/ci.yml:46`
- `.github/workflows/commitlint.yml:12`
- `.github/workflows/commitlint.yml:15`
- `.github/workflows/e2e.yml:11`
- `.github/workflows/e2e.yml:15`
- `.github/actions/setup-node/action.yml:4`
- `.github/actions/setup-node/action.yml:6`

### missing-permissions (severity: medium)

ci.yml has no top-level `permissions:` key and the `build_and_test` job has no job-level `permissions:` key. This means the job runs with the default (potentially broad) token permissions. Only the `publish` job has explicit permissions.

Locations:

- `.github/workflows/ci.yml:1`

### missing-permissions (severity: medium)

commitlint.yml has no top-level `permissions:` key and the `commitlint` job has no job-level `permissions:` key. The workflow runs on `pull_request` events with default (potentially broad) token permissions.

Locations:

- `.github/workflows/commitlint.yml:1`

### missing-permissions (severity: medium)

e2e.yml has no top-level `permissions:` key and the `test` job has no job-level `permissions:` key. The workflow runs with default (potentially broad) token permissions.

Locations:

- `.github/workflows/e2e.yml:1`

## Iteration Notes

### Iteration 1

**Fixes applied:** unpinned-uses, missing-permissions

**Notes:**

Pinned all unpinned action references to full 40-character commit SHAs: actions/checkout@v4.1.7 → 692973e3d937129bcbf40652eb9f2f61becf3332, wagoid/commitlint-github-action@v6 → b948419dd99f3fd78a6548d48f94e3df7f6bf3ed, actions/create-github-app-token@v1 → d72941d797fd3113feb6b93fd0dec494b13a2547, pnpm/action-setup@v4.0.0 → fe02b34f77f8bc703788d5817da081398fad5dd2, actions/setup-node@v4.0.3 → 1e60f620b9541d16bece96c5465dc8ee9832be0b. Added top-level `permissions: {}` to ci.yml, commitlint.yml, and e2e.yml. Added job-level `permissions: contents: read` to build_and_test (ci.yml) and test (e2e.yml) jobs. Added `permissions: contents: read, pull-requests: read` to commitlint job (commitlint.yml needs pull-requests: read for wagoid/commitlint-github-action to read PR commits). The existing publish job permissions (contents: write, issues: write, pull-requests: write) were preserved.

