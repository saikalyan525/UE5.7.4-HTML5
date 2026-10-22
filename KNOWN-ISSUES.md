# Known Issues and Experimental Boundaries

This port is not production-ready and has not completed broad browser, hardware, project, or clean-install validation.

## Current validation boundary

- Compilation, cooking, packaging, WebAssembly initialization, selected Unreal startup stages, WebGL 2 initialization, and selected map-loading paths have been exercised during development.
- Independent clean-checkout reproducibility is still being tested.
- No fresh-checkout end-to-end acceptance result currently demonstrates a visible Third Person scene, 60 seconds of stable ticking, and verified keyboard/mouse gameplay.
- Results from one project or browser version do not establish compatibility for another.

## WebGL 2 renderer limits

WebGL 2 exposes GLSL ES 3.0 but not the general compute-shader capabilities expected by many UE5 renderer paths. HTML5 must not select compute permutations that are unavailable for `GLSL_ES3_0_HTML5`.

Feature paths requiring guards or project-specific evaluation include:

- ShaderPrint compute uploads;
- mobile hierarchical Z-buffer construction;
- Temporal AA compute paths;
- GPU Scene and instance-culling compute work;
- Niagara GPU simulation;
- sparse/virtual texture compute uploads;
- real-time reflection and shadow permutations.

Nanite, Lumen, hardware ray tracing, virtual shadow maps, and other compute-heavy modern UE5 features should be considered unavailable unless independently demonstrated. Do not globally suppress shader-validity assertions to hide missing permutations.

## JSPI

- JSPI is experimental and browser-dependent.
- The implementation makes `main` and `__wasm_call_ctors` promising exports.
- Any exported/native entry path that reaches a suspending import must be invoked through a compatible promising export or it may throw `SuspendError`.
- Synchronization and sleep operations must not be replaced with unsafe no-ops.

## Asyncify

- Whole-module Asyncify instrumentation can take hours and consume substantial RAM.
- The observed default 4 KiB Asyncify stack was insufficient for Unreal startup; the branch uses 16 MiB.
- An Asyncify package may still enter a sustained CPU loop before its first visible frame.

## WebAssembly memory and ABI

- Memory64/wasm64 narrows browser compatibility.
- Browser per-tab and address-space limits can be lower than configured Emscripten maxima.
- Heap-cookie, alignment, allocator, and ABI failures must be diagnosed rather than disabled.
- Toolchain or character-width changes can invalidate generated objects and produce misleading link errors when stale objects remain.

## Cooking, shaders, and browser filesystems

- HTML5 shader libraries must use compression supported by the browser runtime.
- Stale shader compression or shader-platform data may survive an incremental cook.
- IoStore, package mounting, save games, and asynchronous browser filesystem behavior remain experimental.
- Proprietary Epic-bundled technology such as Oodle must remain inside the licensed Unreal Engine distribution boundary.

## Hosting

- Precompressed files require correct `Content-Encoding` and MIME headers.
- Pthread builds require cross-origin isolation.
- Static hosts that cannot set COOP/COEP or correct gzip metadata may be unsuitable.
- Large WebAssembly and data payloads may exceed host, CDN, cache, browser, or network limits.
