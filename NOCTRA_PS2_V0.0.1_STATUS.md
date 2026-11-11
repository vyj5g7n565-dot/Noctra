# NOCTRA PS2 — technical base v0.0.1

## Current scope

This is a technical preparation of the upstream Play! PlayStation 2 emulator for an iOS build. The first milestone deliberately keeps the upstream game-library screen and emulator controls: the priority is proving that the emulator can boot a legally obtained game image on the target iPad before designing a new interface.

## What is already present in upstream source

- A native iOS application under `Source/ui_ios/`.
- Game-library scanning and launching in `CoverViewController.mm`.
- OpenGL ES rendering, with optional Vulkan source when the Vulkan SDK is available.
- An AltServer JIT service (`AltServerJitService.mm`) and a JIT availability check before entering the emulator.
- Built-in high-level BIOS emulation; an external BIOS image is not required by Play!.
- The upstream project explicitly warns that launching a game without JIT can crash.

## Changes in this package

- `installer_ios/build_ipa.sh` now uses a valid Bash shebang, resolves paths independently of the caller's working directory, removes stale Payload output, copies the built app bundle, creates the IPA, and checks ZIP integrity.
- `installer_ios/configure_ios.sh` provides a reproducible arm64 iOS CMake configuration and fails early with a clear message when macOS, Xcode, CMake, or source dependencies are missing.

## Important build limitation in the supplied ZIP

The archive contains `.gitmodules`, but the six Git submodule directories are empty. In particular, the iOS toolchain file under `deps/Dependencies/cmake-ios/ios.cmake` is absent. The exact submodule revisions are also not recorded in this ZIP because it has no `.git` metadata. Therefore this archive cannot be compiled as-is, and this environment is Linux without Apple's iOS SDK/Xcode. No IPA or runtime performance result is claimed.

For a reproducible iOS build, obtain the upstream repository as a Git checkout with recursive submodules, then apply these package changes. The configure script checks for the required dependency files before running CMake.

## JIT and performance expectations

JIT is not an optional speed tweak for this upstream iOS port: the upstream README states that game launch without JIT can crash. The app already contains an AltServer-based JIT activation path. Actual JIT activation depends on the signing/install method and a compatible JIT-enabling workflow on the user's device. This package does not bypass iOS security or guarantee JIT activation.

The build is configured for arm64 and Release. That is a sensible starting point for an Apple Silicon iPad, not proof of full-speed PS2 emulation. Game compatibility and frame rate vary substantially by title and require device testing.

## User content

No commercial game images are included. Import only game images the user is legally entitled to use. Play!'s built-in BIOS emulation means a separate BIOS download is not needed.
