# UNS DataHub Bootstrap

This public repository distributes the minimal `uns-bootstrap` installers and
cross-platform binaries used to acquire a version-matched UNS DataHub runtime.
The complete runtime release may remain private.

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

## Bootstrap and Runtime versions

The bootstrap and Runtime use independent version lines. `uns-bootstrap
version` prints both the bootstrap version and the default Runtime release
embedded in that bootstrap build. A bootstrap `1.x` version and a Runtime `7.x`
version are therefore expected and should not be compared with each other.

Re-run the public platform installer from the Install section to refresh an
older bootstrap binary. This replaces only `uns-bootstrap`; it does not replace
an existing Runtime directory.

## Existing Runtime directory

Re-running `uns-bootstrap install` safely reuses the target when it contains
the same verified Runtime version and resumes the setup wizard. Current
releases report the installed and requested Runtime versions when possible if
the target contains a different or unverifiable Runtime. They then print safe
commands for a side-by-side installation or a rename-and-retry without changing
local files.

Older bootstrap releases may print only `runtime target must be absent or
empty`. Refresh the bootstrap binary, then retry the installation to receive
the more specific guidance.

Do not delete or overwrite `.env`, `.secrets`, configuration, or other local
state to bypass this check. Compare the installed Runtime `VERSION` file with
the `default runtime` value printed by `uns-bootstrap version`, not with the
bootstrap's own version. See [Existing Runtime](https://www.uns-datahub.com/docs/#existing-runtime)
for the cross-platform check.

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
