# WPCodeSpace OpenVSCode Build Validation

## Status

Local macOS build validation was attempted after the first WPCodeSpace Chat composer source customization.

## Environment

- Node: 22.22.0
- npm: 10.9.4
- Repository .nvmrc: 22.22.0
- Platform: macOS arm64

## Commands attempted

- npm ci
- npm run compile-web

## Result

npm ci failed while rebuilding the native dependency:

- @vscode/spdlog
- node-gyp rebuild

The failure occurred during C++ compilation against Electron headers.

Because npm ci failed, node_modules/gulp/bin/gulp.js was not installed, so npm run compile-web could not run.

## Interpretation

This does not prove that the WPCodeSpace Chat source customization is invalid.

It proves that the local macOS dependency installation/build environment is not currently suitable for validating the OpenVSCode web workbench build.

## Required next validation path

Use a Linux/Docker or CI-based build environment matching the intended production image target.

The next validation phase should verify:

1. Dependency installation succeeds in Linux.
2. npm run compile-web succeeds.
3. The OpenVSCode web workbench output is regenerated from source.
4. The WPCodeSpace Chat copy changes appear in the running OpenVSCode Chat panel.
5. No generated runtime files are patched manually.

## Do not do

- Do not patch generated workbench.js.
- Do not patch nls.messages.js.
- Do not treat the failed local macOS build as a product runtime failure.
- Do not proceed to WPCodeSpace Docker image consumption until a Linux/CI build path passes.

## Linux CI validation success

The WPCodeSpace OpenVSCode Linux build validation workflow has passed after adding the required native Linux build dependencies.

Validated in GitHub Actions on Linux:

- npm ci
- npm run compile-web
- no tracked generated workbench.js or nls.messages patching required

This establishes Linux CI as the supported validation path for WPCodeSpace OpenVSCode source customization.

Next validation phase:

1. Identify the correct OpenVSCode server/image build command.
2. Produce a pinned WPCodeSpace OpenVSCode image from this fork.
3. Run the image locally.
4. Verify the WPCodeSpace Chat copy appears in the running OpenVSCode Chat panel.
5. Only then update the WPCodeSpace repo to consume the pinned image.
