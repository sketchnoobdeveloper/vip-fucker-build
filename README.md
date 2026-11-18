# vip-fucker-build

Public GitHub Actions runner for the **private** VIP Fucker Android source repository.

This repository contains **no application source code** — only a workflow and this
document. The workflow checks out the private source with a read-only, fine-grained
token, discovers the build requirements from the source repository's own files,
builds the debug APK, and uploads only the APK and its checksum as an artifact.

## Repositories

| Repository | Visibility | Role |
| --- | --- | --- |
| `sketchnoobdeveloper/VIP-Fucker` | **Private** | Android source (Kotlin, AGP, Gradle files) |
| `sketchnoobdeveloper/vip-fucker-build` | **Public** | This repo — Actions workflows only |

## Required secret

`PRIVATE_REPO_TOKEN` — a **fine-grained personal access token** with:

- Resource owner: `sketchnoobdeveloper`
- Repository access: **Only select repositories** → `VIP-Fucker`
- Repository permissions: **Contents: Read-only**
- No other repository permissions; no organisation permissions.

Set it in this repository at **Settings → Secrets and variables → Actions → New
repository secret**. The token value is never printed by the workflow.

## Running a build

Use **Actions → Build VIP Fucker (private source) → Run workflow**. The optional
`source_ref` input selects a branch, tag, or commit in the private source repo
(default: `main`).

The workflow:

1. validates that the secret exists (without revealing it),
2. checks out the private source with `persist-credentials: false`,
3. discovers Java, Gradle, compileSdk and minSdk from the project's own files,
4. sets up the matching JDK/Gradle and Android SDK packages,
5. runs `:app:assembleDebug`,
6. uploads `app-debug.apk` and `SHA256SUMS.txt` as a workflow artifact.

## Notes

- No private source is copied into this repository, its commits, its logs (only
  version metadata is printed during detection), or its artifacts.
- Artifacts on a public repository are downloadable by anyone with read access to
  this repository. Do not treat them as private storage.
- Nothing here publishes a GitHub Release or changes the source repository's
  visibility.
