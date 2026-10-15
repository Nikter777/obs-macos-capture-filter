# OBS macOS Capture Filter

[English](README.md) | [Русский](README.ru.md)

> Experimental privacy-focused screen capture modifications for OBS Studio on macOS, built using Apple's ScreenCaptureKit.

## Overview

OBS macOS Capture Filter is an experimental modification of OBS Studio's native `mac-capture` plugin.
The project explores application-level filtering during screen recording on macOS, allowing selected applications to remain visible on the user's display while being excluded from the captured video.

It also introduces screenshot-aware capture behavior to prevent the macOS screenshot selection interface from appearing in recordings.

The implementation is based on **OBS Studio 32.0.4**.

## Features

### Application Exclusion

Selected applications can be excluded from screen recordings using Apple's ScreenCaptureKit API.

The following applications have been successfully tested:

| Application | Bundle Identifier | Status |
|---|---|---|
| ChatGPT Classic | `com.openai.chat` | Working |
| AdGuard VPN | `com.adguard.mac.vpn` | Working |

Excluded applications remain visible on the actual macOS desktop but do not appear as normal application windows in the OBS recording.

Application identifiers are currently hardcoded in the source code.

### Screenshot-Aware Capture

The modification also includes experimental handling of the macOS screenshot shortcut:

`Command + Shift + 4`

When the screenshot selection interface is activated, the captured image temporarily freezes.

This prevents the screenshot selection process from being displayed normally in the recording.

**Note:** This behavior is experimental and may affect capture smoothness.

### Native OBS Integration

The implementation modifies OBS Studio's existing macOS capture plugin rather than relying on an external screen-recording application.

Technologies involved include:

- Objective-C
- C
- Apple ScreenCaptureKit
- Core Graphics
- OBS Studio
- CMake

## How It Works

OBS Studio uses Apple's ScreenCaptureKit framework to capture screen content on macOS.

This project modifies the configuration of `SCContentFilter` within the native `mac-capture` plugin.

The filter is configured to exclude selected applications based on their bundle identifiers.

Additional logic monitors screenshot-related activity and temporarily changes capture behavior while the macOS screenshot interface is active.

The modifications are concentrated in three OBS Studio source files:

```text
plugins/mac-capture/mac-sck-common.h
plugins/mac-capture/mac-sck-common.m
plugins/mac-capture/mac-sck-video-capture.m
```

## Known Limitations

This project is an experimental modification, not a complete privacy or security solution.

- **Dock visibility:** The macOS Dock remains visible in recordings, even when exclusion of its application identifier is attempted.
- **Hardcoded exclusions:** Application identifiers must currently be changed in the source code.
- **Screenshot freezing:** The recording temporarily freezes during screenshot selection.
- **Compatibility:** The implementation is based on OBS Studio 32.0.4. Compatibility with other OBS Studio and macOS versions has not been verified.
- **Privacy limitations:** The modification does not guarantee that all system UI elements, notifications, application indicators, or sensitive information will be hidden.

Always verify the recorded output before relying on application exclusion.

## Project Status

**Experimental / Proof of Concept**

Confirmed behavior in the developer's environment:

- ChatGPT Classic is excluded from recordings.
- AdGuard VPN is excluded from recordings.
- Screenshot selection triggers temporary capture freezing.
- The macOS Dock remains visible.

The project is intended for experimentation, technical learning, and exploration of macOS screen capture APIs.

## Installation and Build

Build and installation instructions will be added after the source patch and reproducibility checks are completed.

The project is based on the OBS Studio 32.0.4 source code.

## Demo

A demonstration video showing application exclusion, screenshot-aware capture behavior, and known limitations is planned.

## Upstream Project

This project is based on the open-source [OBS Studio](https://github.com/obsproject/obs-studio).

OBS Studio is developed by the OBS Project and its contributors.

This repository is an independent experimental modification and is not affiliated with or officially supported by the OBS Project.

## License

Licensing information for the OBS-derived modifications will be added before the source patch is published.
