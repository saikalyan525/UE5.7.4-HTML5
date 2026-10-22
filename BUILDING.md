# Building

Use an appropriate Visual Studio Developer PowerShell and activate the Emscripten 4.0.12 environment before building HTML5 targets.

## Build and validate the Windows editor first

For a project with a native editor target:

```powershell
Engine\Build\BatchFiles\Build.bat ProjectNameEditor Win64 Development `
  -Project="C:\Path\ProjectName.uproject" -WaitMutex -NoUBA
```

Open the project and resolve missing-module or plugin failures before attributing them to HTML5.

## Build the HTML5 target

```powershell
Engine\Build\BatchFiles\Build.bat UnrealGame HTML5 Development `
  -Project="C:\Path\ProjectName.uproject" -WaitMutex -NoUBA -HTML5JSPI
```

`-NoUBA` is recommended for the experimental toolchain because Emscripten launches Python, Node, LLVM, and Binaryen child processes outside a typical native compilation flow.

## Async runtime

JSPI is the intended default. The branch's JSPI link configuration includes:

```text
-s JSPI=1
-s JSPI_EXPORTS=["main","__wasm_call_ctors"]
```

`__wasm_call_ctors` must be a promising export because Unreal static constructors can reach imports that suspend.

The experimental fallback uses Asyncify with a larger stack:

```text
-s ASYNCIFY=1
-s ASYNCIFY_STACK_SIZE=16MB
```

Asyncify can instrument a large part of the engine module. Final linking may take hours and consume substantial RAM; process existence or high CPU alone is not proof of forward progress.

## Object consistency

Regenerate the UnrealBuildTool link action rather than maintaining an object list manually. Do not mix:

- unity and non-unity objects;
- objects produced with different ABI choices;
- wasm32 and wasm64 objects;
- objects from different Emscripten overlays;
- stale output produced before a toolchain or linker-runtime change.

After a failure, identify the first compiler or linker error and preserve successful outputs. Avoid clearing all caches or rebuilding the engine without evidence.

## What a successful build proves

Exit code 0 proves that the requested target compiled and linked. It does not prove cooking, packaging, WebAssembly instantiation, Unreal startup, map rendering, ticking, or input.
