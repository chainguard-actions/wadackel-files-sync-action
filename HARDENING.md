<!-- markdownlint-disable -->

# Hardening Report: wadackel--files-sync-action/v3.2.0

> This file was generated automatically by the hardening agent.

**Policy SHA:** `d636be7e43ef829af6e853da6b3c7566db9f72fe`

**Test Policy SHA:** `843adf9e4b8f85d0c08b27b9d0b09dd094b54702`

**Harden Agent Version:** `2`

Action **wadackel--files-sync-action/v3.2.0** was hardened automatically. 2 finding(s) were identified and resolved across 1 iteration(s).

## Findings Fixed

### unpinned-uses (severity: high)

Multiple workflow files and the composite action reference GitHub Actions by mutable version tags instead of immutable 40-character commit SHAs. This exposes the workflow to supply-chain attacks if a tag is moved or a repository is compromised.

Failing references:
- .github/workflows/ci.yml: `actions/checkout@v4.1.7` (two occurrences)
- .github/workflows/e2e.yml: `actions/checkout@v4.1.7`, `actions/create-github-app-token@v1`
- .github/workflows/commitlint.yml: `actions/checkout@v4.1.7`, `wagoid/commitlint-github-action@v6`
- .github/actions/setup-node/action.yml: `pnpm/action-setup@v4.0.0`, `actions/setup-node@v4.0.3`

All should be pinned to full 40-character SHA digests (e.g. `actions/checkout@11bd71901bbe5b1630ceea73d27597364c9af683 # v4.1.7`).

Locations:

- `.github/workflows/ci.yml:14`
- `.github/workflows/ci.yml:47`
- `.github/workflows/e2e.yml:10`
- `.github/workflows/e2e.yml:15`
- `.github/workflows/commitlint.yml:9`
- `.github/workflows/commitlint.yml:12`
- `.github/actions/setup-node/action.yml:4`
- `.github/actions/setup-node/action.yml:6`

### missing-permissions (severity: medium)

Three workflow files lack a top-level `permissions:` block, and not every job within them defines its own `permissions:` block. Without explicit permissions, jobs inherit the default token permissions (which may be broad), violating the principle of least privilege.

- `.github/workflows/ci.yml`: No top-level `permissions:`. The `build_and_test` job has no `permissions:` key (only the `publish` job does).
- `.github/workflows/e2e.yml`: No top-level `permissions:` and no job-level `permissions:` on the `test` job.
- `.github/workflows/commitlint.yml`: No top-level `permissions:` and no job-level `permissions:` on the `commitlint` job.

Each job should declare the minimal permissions it requires (e.g. `permissions: read-all` or specific scopes).

Locations:

- `.github/workflows/ci.yml:1`
- `.github/workflows/e2e.yml:1`
- `.github/workflows/commitlint.yml:1`

## Iteration Notes

### Iteration 1

**Fixes applied:** unpinned-uses, missing-permissions

**Notes:**

Pinned all action references to full 40-character SHAs: actions/checkout@v4.1.7 → 692973e3d937129bcbf40652eb9f2f61becf3332, actions/create-github-app-token@v1 → d72941d797fd3113feb6b93fd0dec494b13a2547, wagoid/commitlint-github-action@v6 → b948419dd99f3fd78a6548d48f94e3df7f6bf3ed, pnpm/action-setup@v4.0.0 → fe02b34f77f8bc703788d5817da081398fad5dd2, actions/setup-node@v4.0.3 → 1e60f620b9541d16bece96c5465dc8ee9832be0b. Added minimal permissions blocks to all jobs lacking them: build_and_test (contents: read), e2e test (contents: read), commitlint (contents: read, pull-requests: read). The publish job already had explicit permissions and was left unchanged.

