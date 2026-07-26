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

Docker or Podman registry authentication is separate from GitHub runtime
access.

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
