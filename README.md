# Atlantis release builder (public)

This repository only contains CI configuration. The application source stays in
the private repository `simtelboy/atlantis-android` and is checked out at build
time with a fine-grained personal access token.

## Required secret

| Secret | Value |
|---|---|
| `PRIVATE_REPO_TOKEN` | Fine-grained PAT, **Contents: Read-only** on `simtelboy/atlantis-android` only, no expiry required |

The token never appears in this repository; anyone able to push to this public
repository could read workflow changes, so keep write access limited to the
owner.

## What it does

- Daily at 04:23 UTC (and on manual dispatch) it asks GitHub for the latest
  `klzgrad/naiveproxy` release tag.
- If no release named `atlantis-<tag>` exists yet, it builds the APK from the
  private source with that naiveproxy core and publishes:
  - Release tag: `atlantis-v154.0.8037.49-5`
  - `versionName`: the naiveproxy tag (Chromium version + naive revision)
  - Debug-signed APK, installable directly.
- When naiveproxy publishes a new release, the next daily run builds a new
  Atlantis release automatically.

`klzgrad/naiveproxy` itself is not a cron job: its workflow runs on pushes to
`master` and on published releases, and the maintainer updates
`CHROMIUM_VERSION` when importing a new Chromium. Polling its releases is
therefore the correct way to follow it.
