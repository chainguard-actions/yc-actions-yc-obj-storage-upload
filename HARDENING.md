<!-- markdownlint-disable -->

# Hardening Report: yc-actions--yc-obj-storage-upload/v2.3.0

> This file was generated automatically by the hardening agent.

**Policy SHA:** `d636be7e43ef829af6e853da6b3c7566db9f72fe`

**Test Policy SHA:** `843adf9e4b8f85d0c08b27b9d0b09dd094b54702`

**Harden Agent Version:** `2`

Action **yc-actions--yc-obj-storage-upload/v2.3.0** was hardened automatically. 2 finding(s) were identified and resolved across 1 iteration(s).

## Findings Fixed

### missing-permissions (severity: medium)

The workflow file test.yml has no top-level `permissions:` key and no job-level `permissions:` key on any of its jobs. Without explicit permissions, the GITHUB_TOKEN is granted its default (potentially broad) permissions, which may include write access to repository contents.

Locations:

- `.github/workflows/test.yml:1`

### unpinned-uses (severity: high)

Multiple `uses:` references in workflow files are pinned to mutable version tags (@v4) rather than immutable full 40-character SHA commit digests. This exposes the workflow to supply-chain attacks if the referenced action tag is moved or compromised. Failing references:
- .github/workflows/check-dist.yml: `actions/checkout@v4`, `actions/setup-node@v4`, `actions/upload-artifact@v4`
- .github/workflows/test.yml: `actions/checkout@v4`

Locations:

- `.github/workflows/check-dist.yml:24`
- `.github/workflows/check-dist.yml:29`
- `.github/workflows/check-dist.yml:50`
- `.github/workflows/test.yml:11`

## Iteration Notes

### Iteration 1

**Fixes applied:** missing-permissions, unpinned-uses

**Notes:**

1. Added `permissions: {}` top-level block to .github/workflows/test.yml to restrict GITHUB_TOKEN to no permissions by default. 2. Pinned all unpinned `uses:` references to full 40-character SHA digests: actions/checkout@v4 → @11d5960a326750d5838078e36cf38b85af677262 (test.yml and check-dist.yml), actions/setup-node@v4 → @49933ea5288caeca8642d1e84afbd3f7d6820020 (check-dist.yml), actions/upload-artifact@v4 → @ea165f8d65b6e75b540449e92b4886f43607fa02 (check-dist.yml). All original tag names preserved as inline comments for readability.

