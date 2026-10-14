# Procket releases

Signed builds of the Procket client for macOS, Windows and Linux.

[Procket](https://procket.dev) records the prompts engineers send to their AI
coding tools, ties them to the commits those prompts produced, and shows them on
the pull request so they can be reviewed alongside the code.

This repository holds release files only. The source is not public. Every
release on the [Releases](https://github.com/procket-dev/procket-releases/releases)
page is also published at `https://get.procket.dev/<version>/`.

## Install

| Platform | Command |
|---|---|
| macOS, Linux | `curl -fsSL https://get.procket.dev/install.sh \| sh` |
| macOS, Linux (Homebrew) | `brew install procket-dev/tap/procket` |
| Windows (PowerShell) | `irm https://get.procket.dev/install.ps1 \| iex` |
| Windows (winget) | `winget install Procket.Procket` |
| Windows (Scoop) | `scoop bucket add procket https://github.com/procket-dev/scoop-bucket` then `scoop install procket` |
| Windows (Chocolatey) | `choco install procket` |

Then run `procket setup`. The [install guide](https://procket.dev/install) has
the details, including signed MSI and pkg installers for Intune, Jamf and
similar tools, deb and rpm packages, and the dev container feature.

## What each release contains

| File | What |
|---|---|
| `procket_<os>_<arch>.tar.gz`, `.zip` | The client for darwin, linux and windows on amd64 and arm64 |
| `procket_<version>_x64.msi`, `_arm64.msi` | Windows installers |
| `procket_<version>.pkg` | macOS installer |
| `procket.<version>.nupkg` | Chocolatey package, for internal feeds |
| `*.deb`, `*.rpm` | Linux packages |
| `checksums.txt`, `installers-checksums.txt` | SHA-256 of every file above |
| `*.sig`, `*.pem` | Sigstore signatures and certificates for the checksum files |
| `*.release.sig` | The Procket release key's signature, which `procket update` checks |
| `*.sbom.json` | Software bill of materials for each archive |

Windows binaries and installers are Authenticode-signed. macOS binaries and the
pkg are signed with a Developer ID and notarised by Apple.

## Verify a download

```sh
cosign verify-blob checksums.txt \
  --certificate checksums.txt.pem \
  --signature checksums.txt.sig \
  --certificate-identity-regexp '^https://github.com/procket-dev/' \
  --certificate-oidc-issuer https://token.actions.githubusercontent.com
sha256sum -c checksums.txt --ignore-missing
```

Do the same with `installers-checksums.txt` for the MSI, pkg and Chocolatey
package. The install scripts and `procket update` check checksums before
installing anything.

## Help

- Questions and problems: [rory@borrowd.co.uk](mailto:rory@borrowd.co.uk)
- Security issues: see [SECURITY.md](SECURITY.md). Please do not open a public issue.

## Licence

Procket is proprietary software. See [LICENSE](LICENSE).
