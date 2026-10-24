# AGENTS.md

This repository is a fork of
[myoung34/docker-github-actions-runner](https://github.com/myoung34/docker-github-actions-runner).
It publishes one image: `ghcr.io/rogerxaic/github-runner`. The self-hosted
runners in the gitops repository use it.

## Every change ships as a pull request

A push to `master` publishes the image. Do not push to `master`. Open a pull
request.

## What differs from upstream

- `Dockerfile.ghcr` builds the image on Ubuntu 26.04 in one stage. The
  upstream `Dockerfile` and `Dockerfile.base` are not used for the image.
- `.github/workflows/ghcr.yml` is the only workflow. The upstream workflows
  push to Docker Hub, so this fork deletes them.
- The image sets `DISABLE_AUTO_UPDATE`. A runner that updates itself in a
  container loses its `bin` directory and fails at every restart.
- `ghcr.yml` runs every day. It builds an image when a new release of
  `actions/runner` has no `<version>-ubuntu-26.04` tag yet.
- `entrypoint.sh` removes stale `docker.pid` and `containerd.pid` files
  before it starts `dockerd`.
- `build/config.json` drops tools that the runners do not use.

## Merge upstream

1. Merge `myoung34/master` into a branch.
2. Keep the upstream workflow files deleted. Resolve each modify/delete
   conflict with `git rm`.
3. Replace this file with the fork version if upstream changes `AGENTS.md`.
4. Open a pull request.

The upstream version bump changes `Dockerfile` only. This fork does not need
it, because `ghcr.yml` gets the version from the latest release.
