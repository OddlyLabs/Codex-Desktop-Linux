# Codex Desktop Linux

Automated Debian package releases for Codex Desktop on Linux.

This repository is an automation layer for building and publishing Linux
packages. It does not vendor, fork, or mirror the upstream packaging source.

GitHub Actions checks out
[`ilysenko/codex-desktop-linux`](https://github.com/ilysenko/codex-desktop-linux)
at build time, downloads the official upstream `Codex.dmg`, builds the Linux
app, packages it as a `.deb`, and publishes the result as a GitHub Release
asset.

## Quick Links

| Need | Link |
| --- | --- |
| Download the latest `.deb` | [Latest release](https://github.com/wintopic/Codex-Desktop-Linux/releases/latest) |
| Browse all builds | [Releases](https://github.com/wintopic/Codex-Desktop-Linux/releases) |
| Watch build status | [GitHub Actions](https://github.com/wintopic/Codex-Desktop-Linux/actions/workflows/release-deb.yml) |
| Upstream packaging source | [`ilysenko/codex-desktop-linux`](https://github.com/ilysenko/codex-desktop-linux) |

## Install

Download the `.deb` file from the
[latest release](https://github.com/wintopic/Codex-Desktop-Linux/releases/latest),
then install it with:

```bash
sudo apt install ./Codex-Desktop-Linux-*.deb
```

## How Releases Are Built

The release workflow runs on a schedule and can also be started manually from
the Actions tab.

1. Check out the selected upstream packaging source.
2. Read metadata for the official upstream `Codex.dmg`.
3. Reuse the cached DMG when the upstream artifact has not changed.
4. Build the Linux application from the DMG.
5. Validate required upstream patches.
6. Build and inspect the Debian package.
7. Publish the package, checksums, and release notes to GitHub Releases.

The workflow skips publishing when an identical release tag already exists,
unless a manual run is started with `force=true`.

## Manual Build

Open the
[`Build and release Debian package`](https://github.com/wintopic/Codex-Desktop-Linux/actions/workflows/release-deb.yml)
workflow and choose **Run workflow**.

| Input | Default | Description |
| --- | --- | --- |
| `force` | `false` | Rebuild and update the release even when the release tag already exists. |
| `upstream_ref` | `main` | Branch, tag, or commit from `ilysenko/codex-desktop-linux` to use as the packaging source. |

## Release Naming

Release tags are deterministic and include:

- the official DMG timestamp
- the official DMG SHA-256 short ID
- the upstream packaging source commit

Example shape:

```text
deb-YYYYMMDD-HHMMSS-d<dmg-short-sha>-u<upstream-short-sha>
```

A new package is produced when either the official DMG content changes or the
selected upstream packaging source changes.

## Repository Layout

```text
.
|-- .github/workflows/release-deb.yml  # Scheduled and manual Debian release workflow
|-- .gitignore
`-- README.md
```

## Notes

- Build inputs and fingerprints are recorded in release notes and workflow
  artifacts.
- The generated package is based on the official upstream DMG and the selected
  upstream packaging repository revision.
- This repository only owns the release automation; application behavior and
  packaging internals come from the upstream sources used during the build.
