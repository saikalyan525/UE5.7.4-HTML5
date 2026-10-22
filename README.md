# Unreal Engine 5.7.4 HTML5

Experimental HTML5/WebAssembly platform work for Unreal Engine 5.7.4.

This public repository contains **documentation only**. It intentionally does not contain Unreal Engine source code, Epic dependencies, installed SDKs, Marketplace/Fab content, generated binaries, or packaged games.

> [!IMPORTANT]
> The implementation source is in the licensed Unreal Engine fork:
> **[saikalyan525/UnrealEngine — `UE5.7.4-HTML5`](https://github.com/saikalyan525/UnrealEngine/tree/UE5.7.4-HTML5)**.
> GitHub users must be authorized for [EpicGames/UnrealEngine](https://github.com/EpicGames/UnrealEngine) to access that fork. Without Epic authorization, the link may appear unavailable or return 404.

## Purpose

The project explores restoring a browser target to a modern Unreal Engine source tree while preserving Unreal's normal build, cook, stage, and package workflow. It is intended for engine developers evaluating WebAssembly, WebGL 2, and browser-runtime integration—not as an Epic-supported production platform.

## Technical profile

| Area | Current target |
| --- | --- |
| Unreal base | `5.7.4-release` |
| Exact Epic base commit | `260bb2e1c5610b31c63a36206eedd289409c5f11` |
| Licensed fork branch | `UE5.7.4-HTML5` |
| Source commit verified for this documentation | `1f1ef1ba3b65ffd5701b46b517b0dca4e15719b4` |
| Emscripten | `4.0.12` with the branch's tracked compatibility overlay |
| WebAssembly | Memory64 / wasm64 |
| Graphics | WebGL 2 / GLSL ES 3.0 |
| Async runtime | JSPI by default; Asyncify is an experimental fallback |
| Host currently documented | Windows 10 or later, Visual Studio 2022, Git Bash |

## Architecture

The licensed branch adds an Unreal HTML5 platform extension across several layers:

1. **Platform registration and SDK validation** for UnrealBuildTool, AutomationTool, and Turnkey.
2. **Emscripten toolchain integration** for wasm64 compilation, JavaScript glue, linking, and HTML5-specific libraries.
3. **Cook, stage, and package support** that produces an HTTP-hosted HTML/JavaScript/WebAssembly/data bundle.
4. **Browser runtime integration** for Unreal's allocator, filesystems, timing, synchronization, input, audio, and WebGL 2 RHI path.
5. **Renderer compatibility guards** where WebGL 2 cannot provide UE5 compute-shader features.

## Status

This work is **experimental and incomplete**.

Development builds have exercised compilation, cooking, packaging, WebAssembly instantiation, selected Unreal initialization stages, WebGL 2 initialization, and selected map-loading paths. Those milestones do **not** establish general browser compatibility or a reproducible playable release.

As of this documentation publication:

- A clean-install run from an independent checkout is still being validated.
- A fresh-checkout, end-to-end browser gameplay acceptance test has not passed.
- Browser and project compatibility must be evaluated individually.
- WebGL 2 cannot support many compute-dependent UE5 renderer features.
- JSPI and wasm64 restrict the compatible browser set.
- Asyncify can require extremely long final-link times and substantial memory.

See [Known Issues](KNOWN-ISSUES.md) for the detailed boundary.

## Documentation

- [Installation and access](INSTALLATION.md)
- [Building targets](BUILDING.md)
- [Cooking and packaging](PACKAGING.md)
- [Browser hosting and testing](BROWSER-TESTING.md)
- [Known issues](KNOWN-ISSUES.md)
- [Contributing](CONTRIBUTING.md)

## Access and licensing boundary

Unreal Engine source is governed by Epic Games' license and must remain within Epic's authorized UnrealEngine repository network. This repository does not grant access to Unreal Engine and does not replace Epic's EULA or third-party notices.

To access the implementation:

1. Link your Epic Games and GitHub accounts using Epic's documented process.
2. Confirm that your account can access `EpicGames/UnrealEngine`.
3. Open the [licensed fork branch](https://github.com/saikalyan525/UnrealEngine/tree/UE5.7.4-HTML5).

Do not publicly mirror the licensed engine source or proprietary Epic-bundled components.

## No support claim

This is a community experiment. It is not affiliated with, endorsed by, or supported by Epic Games. “Unreal” and “Unreal Engine” are trademarks or registered trademarks of Epic Games, Inc.
