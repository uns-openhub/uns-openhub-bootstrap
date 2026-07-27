# UNS DataHub Bootstrap

This public repository distributes the minimal `uns-bootstrap` installers and
cross-platform binaries used to acquire a version-matched UNS DataHub runtime.
The complete runtime release may remain private.

The bootstrap source is maintained in the private
`uns-datahub/uns-datahub-tools` repository. This repository intentionally
contains only public distribution documentation and immutable GitHub Release
assets. It contains no runtime payloads, controller source, credentials, or
private configuration.

## Install

macOS and Linux:

```sh
curl -fsSL \
  https://github.com/uns-datahub/uns-datahub-bootstrap/releases/latest/download/install.sh |
  sh

"$HOME/.local/bin/uns-bootstrap" install
```

Windows PowerShell:

```powershell
Invoke-WebRequest `
  https://github.com/uns-datahub/uns-datahub-bootstrap/releases/latest/download/install.ps1 `
  -OutFile install.ps1
.\install.ps1
& "$HOME\.local\bin\uns-bootstrap.exe" install
```

The public installer verifies the selected platform binary with SHA-256. The
Go bootstrap then tries the matching runtime release anonymously. When that
runtime is private, it asks for a GitHub token using a hidden prompt and does
not store it.

Use a fine-grained, expiring token restricted to the private runtime
repository with read-only **Contents** permission. For unattended hosts,
provide `UNS_GITHUB_TOKEN` through the host's secret mechanism rather than a
command-line argument.

Docker or Podman with Compose is required to run the Runtime. Complete
installation, configuration, startup, and operating instructions are in the
Runtime `README.md` included with the downloaded bundle.

The permanent documentation entry point is
[www.uns-datahub.com/docs/](https://www.uns-datahub.com/docs/).

Docker or Podman registry authentication is separate from GitHub runtime
access.

## Existing Runtime directory

Re-running `uns-bootstrap install` safely reuses the target when it contains
the same verified Runtime version and resumes the setup wizard. If the target
contains a different Runtime version or cannot be verified, the bootstrap
stops with `runtime target must be absent or empty` before changing local
files.

Do not delete or overwrite `.env`, `.secrets`, configuration, or other local
state to bypass this check. Compare the installed `VERSION` file with
`uns-bootstrap version`, then either install to another empty directory with
`--dir` or move the current Runtime to a unique versioned backup before
installing again. See [Resolve an existing Runtime target](https://www.uns-datahub.com/docs/#troubleshooting)
for safe macOS, Linux, and Windows commands.

## Offline installation

Transfer the private runtime archive together with its sibling `.sha256` file,
then run:

```sh
uns-bootstrap install \
  --offline uns-datahub-runtime-<version>-offline.tar.gz
```

The bootstrap refuses non-empty destinations, links, path traversal, checksum
mismatches, and runtime metadata that does not match the requested immutable
version.