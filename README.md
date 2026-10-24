# OBS macOS Capture Filter

[English](README.md) | [Русский](README.ru.md)

> An experimental privacy-focused modification of OBS Studio for macOS, built using Apple's ScreenCaptureKit.

**OBS macOS Capture Filter** is a proof-of-concept project that modifies OBS Studio's native `mac-capture` plugin to exclude selected applications from screen recordings.

The excluded applications remain visible on the actual macOS desktop while their windows are omitted from the captured video.

The project also explores screenshot-aware capture behavior to prevent the macOS screenshot selection interface from appearing normally in recordings.

**Based on OBS Studio 32.0.4.**

## Features

### Application Exclusion

The modification uses Apple's ScreenCaptureKit framework to exclude applications from screen recordings based on their bundle identifiers.

The following applications have been successfully tested:

| Application | Bundle Identifier | Status |
|---|---|---|
| ChatGPT Classic | `com.openai.chat` | Working |
| AdGuard VPN | `com.adguard.mac.vpn` | Working |

Applications are currently specified directly in the source code.

### Screenshot-Aware Capture

The project includes experimental handling of the macOS screenshot shortcut:

`Command + Shift + 4`

When the screenshot selection interface is activated, the capture behavior temporarily changes. Desktop icons may disappear from the captured image and return after the screenshot interface closes.

This behavior prevents the screenshot selection process from appearing normally in the recording.

### Native OBS Integration

The implementation modifies the existing OBS Studio `mac-capture` plugin.

It does not require a separate screen-recording application or an external automation script.

## How It Works

OBS Studio uses Apple's ScreenCaptureKit framework to capture screen content on macOS.

This project modifies the native capture implementation by configuring `SCContentFilter` to exclude selected applications.

Additional logic monitors screenshot-related activity and temporarily changes capture behavior when the macOS screenshot selection interface is active.

The patch modifies three source files:

```text
plugins/mac-capture/mac-sck-common.h
plugins/mac-capture/mac-sck-common.m
plugins/mac-capture/mac-sck-video-capture.m
```

The implementation involves:

- Objective-C and C
- Apple ScreenCaptureKit
- Core Graphics
- macOS system APIs
- CMake and Xcode
- OBS Studio internals

## Project Structure

```text
obs-macos-capture-filter/
├── README.md
├── README.ru.md
├── LICENSE
├── patches/
│   └── obs-32.0.4-macos-capture.patch
└── docs/
    ├── build.md
    └── build.ru.md
```

## Build and Installation

The project provides a source patch targeting **OBS Studio 32.0.4**.

The patch has been checked against the original OBS source files using Git.

The patch has been successfully applied to a fresh OBS Studio 32.0.4 checkout. Both the modified mac-capture plugin and the OBS Studio application have been independently compiled using CMake and Xcode. Runtime testing was also completed using a separately signed test application with a distinct bundle identifier. Exclusion of ChatGPT Classic and AdGuard VPN windows was confirmed.

### Documentation

- [Build instructions (English)](docs/build.md)
- [Инструкция по сборке (Русский)](docs/build.ru.md)

### Source Patch

- [OBS Studio 32.0.4 macOS capture patch](patches/obs-32.0.4-macos-capture.patch)

**Important:** Replacing components inside an existing OBS Studio installation may affect code signing, application updates, permissions, or other functionality.

## Verified Behavior

The following results were observed in the developer's environment:

| Feature | Result |
|---|---|
| ChatGPT Classic exclusion | Working |
| AdGuard VPN exclusion | Working |
| Screenshot selection (`Command + Shift + 4`) | Temporary capture filtering; normal capture resumes afterward |
| macOS Dock visibility | Inconsistent across tests; not reliably excluded |

These results describe the tested environment and do not guarantee identical behavior on other systems.

## Known Limitations

This project is experimental and is not intended to provide complete privacy protection.

- **Dock visibility:** Dock behavior has varied between tests. Its exclusion is not guaranteed.
- **Hardcoded application list:** Changing the excluded applications currently requires modifying the source code.
- **Screenshot selection:** Desktop icons may temporarily disappear from the captured image. Normal capture resumes after the screenshot interface closes.
- **Version compatibility:** The patch targets OBS Studio 32.0.4. Other versions have not been verified.
- **System UI:** Some system elements, notifications, application indicators, or sensitive information may still appear in recordings.
- **Installation:** A general-purpose installer is not provided.

Always review recorded output before relying on application exclusion.

## Demonstration

A 16-second side-by-side demonstration is available in the [v0.1.0 release](https://github.com/Nikter777/obs-macos-capture-filter/releases/tag/v0.1.0).

**[Watch or download the demonstration (MP4)](https://github.com/Nikter777/obs-macos-capture-filter/releases/download/v0.1.0/demo.mp4)**

- **Left — Original Screen:** Standard macOS screen recording showing the actual desktop, including ChatGPT Classic and AdGuard VPN windows.
- **Right — OBS Filtered Capture:** Recording produced by the modified OBS Studio application, with the selected application windows excluded.

The recordings are approximately synchronized. This demonstration illustrates application-window exclusion, not comprehensive privacy protection.

## Project Status

**Experimental / Proof of Concept**

The project was created as an independent technical experiment exploring native macOS screen capture APIs and modifications to OBS Studio.

Future improvements may include configurable application exclusions, additional compatibility testing, and simplified installation.

## License

This repository contains modifications to OBS Studio, which is distributed under the GNU General Public License.

The full text of **GNU GPL version 2** is included in the [LICENSE](LICENSE) file.

Original copyright and licensing notices from OBS Studio and its contributors remain applicable.

## Acknowledgments

This project is based on [OBS Studio](https://github.com/obsproject/obs-studio), developed by the OBS Project and its contributors.

This repository is an independent experimental modification and is not affiliated with or officially supported by the OBS Project.
