<!-- markdownlint-disable -->

# Hardening Report: r0adkll--sign-android-release/v1.0.4

> This file was generated automatically by the hardening agent.

**Policy SHA:** `d636be7e43ef829af6e853da6b3c7566db9f72fe`

**Test Policy SHA:** `843adf9e4b8f85d0c08b27b9d0b09dd094b54702`

**Harden Agent Version:** `2`

Action **r0adkll--sign-android-release/v1.0.4** was hardened automatically. 2 finding(s) were identified and resolved across 1 iteration(s).

## Findings Fixed

### unpinned-uses (severity: high)

The workflow file uses action references pinned to mutable tags/branches rather than immutable full SHA commits. 'actions/checkout@v1' uses a version tag and 'hole19/git-tag-action@master' uses a branch name. Either could be silently updated to point to malicious code. Both should be pinned to a full 40-character hex commit SHA (e.g., actions/checkout@11bd71901bbe5b1630ceea73d27597364c9af683 # v1).

Locations:

- `.github/workflows/release.yml:12`
- `.github/workflows/release.yml:22`

### missing-permissions (severity: medium)

The workflow file '.github/workflows/release.yml' has no top-level 'permissions:' key and the single job 'release' also has no job-level 'permissions:' key. Without explicit permissions, the GITHUB_TOKEN is granted default (potentially broad write) permissions. A minimal permissions block should be added at the top level or per-job level.

Locations:

- `.github/workflows/release.yml:1`

## Iteration Notes

### Iteration 1

**Fixes applied:** unpinned-uses, missing-permissions

**Notes:**

Fixed hardened/action/.github/workflows/release.yml: (1) Pinned actions/checkout@v1 to full SHA 50fbc622fc4ef5163becd7fab6573eac35f8462e and hole19/git-tag-action@master to full SHA 67be507b7332465ace778ee3dad62657482fedfb, preserving original tag/branch names as comments. (2) Added top-level 'permissions: contents: write' block — contents:write is the minimum required for the git-tag-action to create/update repository tags.

