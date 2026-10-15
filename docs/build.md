# Building OBS macOS Capture Filter

This document describes how to apply the experimental capture filter patch to OBS Studio 32.0.4 and build the modified macOS capture plugin.

> **Experimental:** These instructions are based on the developer's existing OBS build environment. A complete clean-build reproduction has not yet been verified.

## Requirements

- macOS
- Xcode and Xcode Command Line Tools
- CMake
- Git
- OBS Studio 32.0.4 source code
- OBS Studio build dependencies

The modification uses Apple's ScreenCaptureKit framework.

The minimum supported macOS version for this modification has not been established.

## 1. Obtain OBS Studio Source Code

Clone the official OBS Studio repository:

```bash
git clone --recursive --branch 32.0.4 https://github.com/obsproject/obs-studio.git obs-filtered
cd obs-filtered
```

OBS Studio has additional build dependencies. Refer to the upstream OBS Studio build documentation when preparing a new build environment.

## 2. Apply the Patch

Download the patch from this repository:

`patches/obs-32.0.4-macos-capture.patch`

Copy it into an accessible location.

From the OBS Studio source directory, run:

```bash
git apply --check /path/to/obs-32.0.4-macos-capture.patch
git apply /path/to/obs-32.0.4-macos-capture.patch
```

The first command checks whether the patch can be applied without modifying files.

The second command applies the modifications.

The patch changes three files:

```text
plugins/mac-capture/mac-sck-common.h
plugins/mac-capture/mac-sck-common.m
plugins/mac-capture/mac-sck-video-capture.m
```

## 3. Configure the Build

The developer's existing build used the following CMake settings:

```text
CMAKE_GENERATOR=Xcode
CMAKE_OSX_DEPLOYMENT_TARGET=12.0
```

These settings describe the original build environment and do not establish the minimum macOS version supported by the modification.

For a new environment, configure OBS Studio according to its upstream macOS build instructions.

The original project's build directory was named `build_macos`.

## 4. Build the macOS Capture Plugin

After configuring the OBS Studio build environment, run:

```bash
cmake --build build_macos --config Debug --target mac-capture -j "$(sysctl -n hw.ncpu)"
```

The developer's build directory contains the plugin at:

```text
build_macos/plugins/mac-capture/Debug/mac-capture.plugin
```

The exact output location may depend on the build configuration.

## 5. Testing

The following behavior was observed in the developer's environment:

| Test | Result |
|---|---|
| ChatGPT Classic application exclusion | Working |
| AdGuard VPN application exclusion | Working |
| Screenshot shortcut (`Command + Shift + 4`) | Temporary capture freezing |
| macOS Dock exclusion | Not working |

The application exclusion list is currently hardcoded in the source code.

## Important Notes

- This is an experimental modification of OBS Studio.
- The patch targets OBS Studio 32.0.4.
- A clean build from these instructions has not yet been verified.
- The presence of a compiled plugin does not guarantee compatibility with other OBS installations.
- Do not overwrite an existing OBS installation without a verified backup and a recovery plan.
- Replacing or re-signing OBS application components may affect code signing, updates, permissions, or other functionality.
- The project does not guarantee complete privacy during screen recording.

## Upstream Project

OBS Studio source code and build documentation:

https://github.com/obsproject/obs-studio
