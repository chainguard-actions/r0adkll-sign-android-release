<!-- markdownlint-disable -->

# Hardening Report: r0adkll--sign-android-release/v1

> This file was generated automatically by the hardening agent.

**Policy SHA:** `d636be7e43ef829af6e853da6b3c7566db9f72fe`

**Test Policy SHA:** `843adf9e4b8f85d0c08b27b9d0b09dd094b54702`

**Harden Agent Version:** `2`

Action **r0adkll--sign-android-release/v1** was hardened automatically. 3 finding(s) were identified and resolved across 1 iteration(s).

## Findings Fixed

### unpinned-uses (severity: high)

The workflow file .github/workflows/release.yml references actions by mutable tags or branch names instead of full 40-character commit SHAs. Failing references: 'actions/checkout@v1' (line 14, line 48), 'pCYSl5EDgo/cat@1.0.0' (line 20), 'hole19/git-tag-action@master' (line 52). These can be silently updated to include malicious code without the workflow noticing.

Locations:

- `.github/workflows/release.yml:14`
- `.github/workflows/release.yml:20`
- `.github/workflows/release.yml:48`
- `.github/workflows/release.yml:52`

### hardcoded-credentials (severity: high)

The workflow file .github/workflows/release.yml contains hardcoded literal credential values. 'keyStorePassword: android' and 'keyPassword: android' appear as plain-text literal strings passed to the Sign APK step (lines 31-32) and Sign AAB step (lines 39-40). These should be stored as GitHub Actions secrets and referenced via ${{ secrets.* }} expressions.

Locations:

- `.github/workflows/release.yml:31`
- `.github/workflows/release.yml:32`
- `.github/workflows/release.yml:39`
- `.github/workflows/release.yml:40`

### missing-permissions (severity: medium)

The workflow file .github/workflows/release.yml has no top-level 'permissions:' key, and neither the 'test' job nor the 'release' job defines its own 'permissions:' block. Without explicit permissions, the workflow inherits the default repository permissions, which may be overly broad (e.g., write access to contents). Explicit minimal permissions should be declared.

Locations:

- `.github/workflows/release.yml:1`

## Iteration Notes

### Iteration 1

**Fixes applied:** unpinned-uses, hardcoded-credentials, missing-permissions

**Notes:**

Fixed .github/workflows/release.yml: (1) Pinned all four action references to full commit SHAs — actions/checkout@v1 → @50fbc622fc4ef5163becd7fab6573eac35f8462e, pCYSl5EDgo/cat@1.0.0 → @264f5b318158276af69bd0a2a9f1e613b2d03ebf, hole19/git-tag-action@master → @67be507b7332465ace778ee3dad62657482fedfb. (2) Replaced hardcoded 'android' keyStorePassword and keyPassword literals in both Sign APK and Sign AAB steps with ${{ secrets.KEY_STORE_PASSWORD }} and ${{ secrets.KEY_PASSWORD }}. (3) Added top-level 'permissions: {}' to deny all by default, plus per-job minimal permissions: 'contents: read' for the test job and 'contents: write' for the release job (needed to push the v1 tag).

