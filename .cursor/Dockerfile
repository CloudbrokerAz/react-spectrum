# Cloud agent base image for react-spectrum.
# Pins Node 24 (.nvmrc) and enables Corepack so Yarn 4.13 (packageManager) resolves.
# Dependencies are installed by the `install` command in environment.json, from the repo root.
FROM node:24-bookworm

# git and sudo are required for Cursor Cloud Agents (repo clone and setup).
RUN apt-get update && DEBIAN_FRONTEND=noninteractive apt-get install -y --no-install-recommends \
    git \
    sudo \
    make \
  && apt-get clean \
  && rm -rf /var/lib/apt/lists/*

RUN corepack enable
