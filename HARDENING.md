<!-- markdownlint-disable -->

# Hardening Report: yc-actions--yc-obj-storage-upload/v2.2.0

> This file was generated automatically by the hardening agent.

**Policy SHA:** `d636be7e43ef829af6e853da6b3c7566db9f72fe`

**Test Policy SHA:** `843adf9e4b8f85d0c08b27b9d0b09dd094b54702`

**Harden Agent Version:** `2`

Action **yc-actions--yc-obj-storage-upload/v2.2.0** was hardened automatically. 2 finding(s) were identified and resolved across 1 iteration(s).

## Findings Fixed

### unpinned-uses (severity: high)

Multiple `uses:` references in workflow files pin to mutable version tags (@v4) rather than immutable 40-character commit SHAs. This exposes the workflow to supply-chain attacks if the referenced action tag is moved or compromised. Affected references: `actions/checkout@v4` (check-dist.yml line 27, test.yml line 13), `actions/setup-node@v4` (check-dist.yml line 31), `actions/upload-artifact@v4` (check-dist.yml line 57). Each should be pinned to a full SHA, e.g. `actions/checkout@11bd71901bbe5b1630ceea73d27597364c9af683 # v4`.

Locations:

- `.github/workflows/check-dist.yml:27`
- `.github/workflows/check-dist.yml:31`
- `.github/workflows/check-dist.yml:57`
- `.github/workflows/test.yml:13`

### missing-permissions (severity: medium)

The workflow file `test.yml` has no top-level `permissions:` block and its only job (`build`) also has no job-level `permissions:` block. Without explicit permissions, the workflow inherits the repository's default token permissions, which may be overly broad (write access to contents, packages, etc.). A minimal `permissions:` block such as `permissions: contents: read` should be added.

Locations:

- `.github/workflows/test.yml:1`

## Iteration Notes

### Iteration 1

**Fixes applied:** unpinned-uses, missing-permissions

**Notes:**

Pinned all mutable action references to full commit SHAs: actions/checkout@v4 → @34e114876b0b11c390a56381ad16ebd13914f8d5 (check-dist.yml line 27 and test.yml line 13), actions/setup-node@v4 → @49933ea5288caeca8642d1e84afbd3f7d6820020 (check-dist.yml line 31), actions/upload-artifact@v4 → @ea165f8d65b6e75b540449e92b4886f43607fa02 (check-dist.yml line 57). Added top-level `permissions: contents: read` block to test.yml to restrict default token permissions.

