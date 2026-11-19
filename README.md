# vc4-apt

Signed apt repository and pip index for the VideoCore IV packages.
Served by GitHub Pages at <https://rughinnit.github.io/vc4-apt>.

| URL | Purpose |
| --- | --- |
| `https://rughinnit.github.io/vc4-apt` | apt source root (`stable main`, arm64) |
| `https://rughinnit.github.io/vc4-apt/key.asc` | archive key (E4CB3BA058DAAB2BE60EF5D6B736BB9A48B94108) |
| `https://rughinnit.github.io/vc4-apt/pip/simple/` | PEP 503 pip index (`vc4infer`) |

## Contents

- `pool/main/v/{vc4top,vc4top-dkms,vc4infer}/*.deb` - package pool (newest only)
- `dists/stable/{InRelease,Release,Release.gpg}` - signed `Release` metadata
- `dists/stable/main/binary-arm64/{Packages,Packages.gz}` - package lists
- `key.asc`, `vc4top-archive-keyring.gpg` - archive key, armored and binary
- `pip/` - wheel and PEP 503 simple index
- `.nojekyll` - serve the tree verbatim, no Jekyll processing

## Install

```sh
curl -fsSL https://rughinnit.github.io/vc4-apt/key.asc \
  | sudo gpg --dearmor -o /usr/share/keyrings/vc4top-archive-keyring.gpg
echo "deb [signed-by=/usr/share/keyrings/vc4top-archive-keyring.gpg] https://rughinnit.github.io/vc4-apt stable main" \
  | sudo tee /etc/apt/sources.list.d/vc4-apt.list
sudo apt update && sudo apt install vc4top vc4top-dkms vc4infer

pip install --index-url https://rughinnit.github.io/vc4-apt/pip/simple/ vc4infer
```

## Publishing

`packaging/publish_apt.sh` in [vc4-stack](https://github.com/rughinnit/vc4-stack)
and `tools/build_apt_repo.sh` in [vc4top](https://github.com/rughinnit/vc4top)
regenerate `Packages`, `Release`, `Release.gpg` and `InRelease` over the whole
pool with `GNUPGHOME=~/.gnupg-vc4top` (key `E4CB3BA0...`), verify them with
`gpgv`, then commit and push this branch.

The old `https://rughinnit.github.io/vc4top` apt/pip tree is **deprecated** but
still published as a fallback.
