# WPCodeSpace OpenVSCode Runtime/Image Build Discovery

## Purpose

Determine the safest runtime/server/image build path for the DXBMARK OpenVSCode fork before WPCodeSpace consumes a pinned OpenVSCode image.

## Current status

No canonical Docker/image build path is proven from repository evidence.

The repository does contain server/runtime archive build candidates, but no repo-owned Docker image build and publish path was proven during discovery.

## Already validated

The following WPCodeSpace OpenVSCode validation has already passed on Linux CI:

- npm ci
- npm run compile-web
- no tracked generated workbench.js or nls.messages patching required

This validates source compilation. It does not validate runtime image creation.

## Candidate runtime/server build commands found

The following candidates were found from repository evidence:

- npm run server:init
- ./scripts/code-server.sh
- npm run compile-web
- npm run gulp vscode-reh-linux-$(VSCODE_ARCH)-min-ci
- npm run gulp vscode-reh-web-linux-$(VSCODE_ARCH)-min-ci

## Evidence

| Area | Finding |
| --- | --- |
| package.json | Defines server:init. |
| scripts/code-server.sh | Launches the server path through out/server-main.js. |
| scripts/code-server.js | Participates in the code-server launch path. |
| package.json | Defines compile-web. |
| .github/workflows/wpcodespace-linux-build-validation.yml | Validates npm ci and npm run compile-web on Linux. |
| build/gulpfile.reh.ts | Defines vscode-reh-* and vscode-reh-web-* packaging task family. |
| product-build-linux-compile.yml | Uses vscode-reh-linux-$(VSCODE_ARCH)-min-ci and vscode-reh-web-linux-$(VSCODE_ARCH)-min-ci style tasks for server archive builds. |

## Docker/image build path

A canonical Docker image build path was not proven.

Discovery did not prove a repo-owned path for:

- docker build
- docker buildx
- ghcr.io publishing
- registry publishing
- pinned WPCodeSpace OpenVSCode image creation

Dockerfiles found during discovery appear to be development or test fixtures, not a canonical production image build path for WPCodeSpace consumption.

## Rejected paths

### Generated bundle patching

Rejected.

WPCodeSpace must not patch:

- workbench.js
- nls.messages.js
- nls.messages.json
- out/ generated runtime bundles

### Immediate WPCodeSpace image consumption

Rejected.

WPCodeSpace must not consume a pinned OpenVSCode image until the runtime/image build path and visual Chat verification both pass.

### Inventing a Dockerfile now

Rejected.

A Dockerfile or image workflow should not be invented until the intended runtime artifact, image target, tag convention, and publish location are chosen.

## Safest next step

Create a small follow-up validation plan for runtime image production.

The next phase should decide:

1. Whether WPCodeSpace should build from OpenVSCode server archive artifacts or directly from source.
2. The intended image name, for example ghcr.io/dxbmark/wpcodespace-openvscode.
3. The tag convention, for example commit SHA or semver-style release tag.
4. The runtime command used inside the image.
5. The local run command used for visual verification.
6. The proof required before WPCodeSpace consumes the image.

## Stop conditions

Stop before implementation if:

- no canonical image target is selected
- no tag convention is selected
- no runtime command is proven
- generated runtime bundles would need manual patching
- WPCodeSpace would need to consume an unverified image
- source customization cannot be observed in a running OpenVSCode Chat panel

## Required proof before WPCodeSpace consumption

Before changing WPCodeSpace to consume a pinned image, the following must be proven:

1. The fork builds a runtime artifact or image successfully.
2. The runtime starts locally.
3. The official OpenVSCode Chat panel opens.
4. WPCodeSpace Chat copy appears in the running Chat panel.
5. No generated runtime file was manually patched.
6. The image is pinned by immutable tag or digest.

## Server archive validation success

The WPCodeSpace OpenVSCode Server Archive Validation workflow passed after preparing the required build metadata file at `out-build/date`.

Validated in GitHub Actions on Linux:

- `npm ci`
- `npm run gulp vscode-reh-linux-x64-min-ci`
- `npm run gulp vscode-reh-web-linux-x64-min-ci`
- no tracked generated `workbench.js` or `nls.messages` patching required

Successful workflow:

- name: `WPCodeSpace OpenVSCode Server Archive Validation`
- run id: `27236526209`
- head SHA: `9e0d98f8af13dd5e36728b9afa97fb73bc0d9375`
- conclusion: `success`

This proves the fork can build the relevant Linux server archive candidates from source.

This still does not create or publish a Docker image. The next phase must define the WPCodeSpace OpenVSCode image build strategy before WPCodeSpace consumes a pinned image.
