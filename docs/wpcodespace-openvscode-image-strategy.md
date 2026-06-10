# WPCodeSpace OpenVSCode Image Strategy

## Purpose

Define the WPCodeSpace OpenVSCode image strategy after proving that the DXBMARK OpenVSCode fork can compile and build Linux server archive candidates from source.

## Current proven state

The following have passed in GitHub Actions on Linux:

- npm ci
- npm run compile-web
- npm run gulp vscode-reh-linux-x64-min-ci
- npm run gulp vscode-reh-web-linux-x64-min-ci
- no tracked generated workbench.js or nls.messages patching required

This proves source compilation and server archive candidate generation.

It does not yet prove Docker image creation, local runtime startup, or visual Chat verification.

## Decision

WPCodeSpace will use a dedicated OpenVSCode image produced from the DXBMARK OpenVSCode fork.

Target image name:

- ghcr.io/dxbmark/wpcodespace-openvscode

Initial tag strategy:

- commit SHA tag for immutable traceability
- optional human-readable phase tag after validation

Example tags:

- ghcr.io/dxbmark/wpcodespace-openvscode:<openvscode-commit-sha>
- ghcr.io/dxbmark/wpcodespace-openvscode:chat-composer-shell-<short-sha>

WPCodeSpace must consume only an immutable tag or digest after runtime validation passes.

## Artifact source

The image build strategy should use the proven Linux server archive candidate path as the runtime source.

Proven candidate commands:

- npm run gulp vscode-reh-linux-x64-min-ci
- npm run gulp vscode-reh-web-linux-x64-min-ci

The exact archive output path must be captured in the next implementation workflow before image creation is finalized.

## Required image validation

Before WPCodeSpace consumes this image, the following must pass:

1. Build the server archive candidates from the fork.
2. Build the OpenVSCode Docker image from the proven archive output.
3. Start the image locally.
4. Open the running OpenVSCode UI.
5. Open the official Chat panel.
6. Verify WPCodeSpace Chat copy appears:
   - Plan mode
   - Describe what to build
   - Build Workspace
   - Review WordPress Project
   - Plan Changes
   - Show Config
7. Confirm no generated runtime bundle was manually patched.
8. Pin the image by immutable tag or digest.

## Explicit non-goals

This phase does not:

- modify WPCodeSpace Dockerfiles
- publish production images
- add provider/backend integration
- add API keys or OAuth
- change Chat behavior beyond already merged source copy
- patch generated workbench.js or nls.messages files

## Stop conditions

Stop before WPCodeSpace consumption if:

- image build fails
- local image startup fails
- official Chat panel does not show the WPCodeSpace copy
- image tag is mutable only
- generated runtime bundles require manual patching
- runtime depends on untracked local state

## Next implementation phase

Add a CI workflow or script that:

1. builds the proven server archive candidates
2. prints the exact archive output paths
3. builds a test image using those outputs
4. optionally uploads the image as an artifact or pushes to GHCR only after credentials and tag policy are confirmed

The first implementation should prefer validation over publishing.
