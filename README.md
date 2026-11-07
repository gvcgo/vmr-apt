# vmr APT repository

Signed APT repository for [`vmr`](https://github.com/gvcgo/version-manager), served from GitHub Pages.

## Install

```bash
curl -fsSL https://gvcgo.github.io/vmr-apt/vmr.gpg \
  | sudo gpg --dearmor -o /usr/share/keyrings/vmr.gpg
echo "deb [signed-by=/usr/share/keyrings/vmr.gpg] https://gvcgo.github.io/vmr-apt stable main" \
  | sudo tee /etc/apt/sources.list.d/vmr.list
sudo apt update
sudo apt install vmr
```

Homebrew/Scoop users get the same binary; vmr installs the SDK versions themselves under `~/.vmr`.

## Layout

- `pool/main/v/vmr/` - `.deb` packages (amd64, arm64); older releases stay in the pool
- `dists/stable/` - `Packages` indexes, `Release`, `InRelease`
- `vmr.gpg` - public signing key

Everything above is written by the vmr release workflow
(`.github/workflows/release.yml`, job `apt`) and signed with the key stored in the
`APT_GPG_PRIVATE_KEY` secret of the version-manager repository. Do not edit it by hand.

## Updating

Each published vmr release appends its `.deb` to the pool and regenerates the indexes, so
`apt update && apt install vmr` picks up the new version. Until the version-manager repository
has `TAP_GITHUB_TOKEN` and `APT_GPG_PRIVATE_KEY` configured, install the `.deb` attached to a
GitHub release instead.
