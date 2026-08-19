<!-- markdownlint-disable -->

# Hardening Report: yc-actions--yc-obj-storage-upload/v3.0.0

> This file was generated automatically by the hardening agent.

**Policy SHA:** `d636be7e43ef829af6e853da6b3c7566db9f72fe`

**Test Policy SHA:** `843adf9e4b8f85d0c08b27b9d0b09dd094b54702`

**Harden Agent Version:** `2`

Action **yc-actions--yc-obj-storage-upload/v3.0.0** was hardened automatically. 2 finding(s) were identified and resolved across 1 iteration(s).

## Findings Fixed

### unpinned-uses (severity: high)

Workflow files reference GitHub Actions using mutable version tags (@v4) instead of immutable full 40-character commit SHAs. This exposes the workflow to supply-chain attacks if the tag is moved to a malicious commit.

Failing references in .github/workflows/check-dist.yml:
- uses: actions/checkout@v4
- uses: actions/setup-node@v4
- uses: actions/upload-artifact@v4

Failing references in .github/workflows/test.yml:
- uses: actions/checkout@v4

Each should be pinned to a full SHA, e.g. `actions/checkout@11bd71901bbe5b1630ceea73d27597364c9af683 # v4`.

Locations:

- `.github/workflows/check-dist.yml:24`
- `.github/workflows/check-dist.yml:29`
- `.github/workflows/check-dist.yml:57`
- `.github/workflows/test.yml:12`

### missing-permissions (severity: medium)

The workflow file test.yml has no top-level `permissions:` key and no job-level `permissions:` key on any of its jobs. Without explicit permissions, the workflow inherits the repository's default token permissions, which may be overly broad (e.g., write access to contents). A minimal `permissions:` block (e.g., `contents: read`) should be added at the top level or on each job.

Locations:

- `.github/workflows/test.yml:1`

## Iteration Notes

### Iteration 1

**Fixes applied:** unpinned-uses, missing-permissions

**Notes:**

Fixed all unpinned action references by pinning to full commit SHAs: actions/checkout@v4 → @34e114876b0b11c390a56381ad16ebd13914f8d5, actions/setup-node@v4 → @49933ea5288caeca8642d1e84afbd3f7d6820020, actions/upload-artifact@v4 → @ea165f8d65b6e75b540449e92b4886f43607fa02. Added top-level `permissions: contents: read` to test.yml to address the missing-permissions finding.

