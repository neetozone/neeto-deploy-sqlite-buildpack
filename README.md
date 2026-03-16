# NeetoDeploy SQLite Buildpack

A [Cloud Native Buildpack](https://buildpacks.io) that installs `sqlite3` and `libsqlite3-dev` for Rails and Node.js applications deployed on neetoDeploy.

## Detection

The buildpack activates automatically when `sqlite3` is found in any of the following files:

- `Gemfile` or `Gemfile.lock`
- `package.json`, `package-lock.json`, or `yarn.lock`

## What it does

Runs `apt-get install sqlite3 libsqlite3-dev` during the build phase and verifies the installation before proceeding.

## Multi-Architecture Support

Supports both `linux/amd64` and `linux/arm64`.

## Building

`scripts/package.sh` packages the buildpack into a `.cnb` archive. Targets are read from `buildpack.toml` automatically if not specified.

```bash
# Package for all targets defined in buildpack.toml
./scripts/package.sh --version 1.0.0

# Package for specific targets
./scripts/package.sh --version 1.0.0 --target linux/amd64 --target linux/arm64

# Package for a single target with an explicit output path
./scripts/package.sh --version 1.0.0 --target linux/amd64 --output build/buildpackage-linux-amd64.cnb
```

Output:

- `build/buildpack.tgz` — buildpack archive (used for publishing)
- `build/buildpackage.cnb` — OCI buildpackage (one file per target if multi-arch)

## Publishing

`scripts/publish.sh` pushes the buildpack to an OCI registry. It reads targets from `buildpack.toml` and creates a multi-arch manifest list when multiple targets are configured.

```bash
./scripts/publish.sh \
  --image-ref 348674388966.dkr.ecr.us-east-1.amazonaws.com/neeto-deploy/buildpacks/sqlite:1.0.0 \
  --archive-path build/buildpack.tgz
```

Run `scripts/package.sh` before publishing.

## Versions

- `sqlite3`: system latest via `apt-get`
