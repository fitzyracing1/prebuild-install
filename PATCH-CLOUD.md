# patch-cloud build of prebuild-install@7.1.3

This tarball was built by patch-cloud from [fitzyracing1/prebuild-install@patch-cloud](https://github.com/fitzyracing1/prebuild-install/tree/patch-cloud),
a fork of [prebuild/prebuild-install](https://github.com/prebuild/prebuild-install). It is **not** an official upstream release.
Package name and version are unchanged from upstream (`prebuild-install@7.1.3`, base `tag v7.1.3`).

## Fix

Require tar-fs ^2.1.4 instead of ^2.0.0 so vulnerable tar-fs (CVE-2025-59343 / GHSA-vj76-c3g6-qr5v, symlink validation bypass) can no longer satisfy prebuild-install's dependency.

- Upstream PR: https://github.com/prebuild/prebuild-install/pull/214
- Upstream issue: https://github.com/prebuild/prebuild-install/issues/213
- Credit: @alichtman (PR #214 author; Co-authored-by in the fork commit)

## Exactly what changed vs upstream

- package.json: "tar-fs": "^2.0.0" -> "^2.1.4" (plus trailing newline). No source changes.
- Note: a fresh install of upstream 7.1.3 already resolves tar-fs 2.1.5; this change matters when an existing lockfile or dedupe keeps an older tar-fs (e.g. 2.1.1) under prebuild-install.
- Added this `PATCH-CLOUD.md` and a `_patchCloud` field in `package.json` (metadata only).

## Verification

- Project with a lockfile pinning tar-fs@2.1.1 under prebuild-install@7.1.3: npm audit reports GHSA-vj76-c3g6-qr5v; after installing this build, npm upgrades tar-fs to >=2.1.4 and npm audit no longer reports it.
- Upstream tape tests: download-test (tar-fs extraction) 35/35, rc 57, util 27, proxy 9, skip 6, log 3 pass. asset-test fails identically with and without this change (pre-existing; Node 20 stream incompatibility in the test harness).

## Opting out

Remove the patch-cloud `registry=` override and reinstall from registry.npmjs.org to get the upstream tarball.
