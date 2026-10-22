# Cooking and Packaging

## Project preparation

Before packaging:

1. Set `GameDefaultMap` to the browser startup map.
2. Add every required map to the maps-to-cook list.
3. Verify the project's GameMode and player/controller assumptions.
4. Disable or replace renderer features that WebGL 2 cannot support.
5. Use the `GLSL_ES3_0_HTML5` shader platform.
6. Select JSPI unless deliberately testing the Asyncify fallback.

Adding a map to the cook list does not make it the startup map.

## Representative BuildCookRun command

```powershell
Engine\Build\BatchFiles\RunUAT.bat BuildCookRun `
  -project="C:\Path\ProjectName.uproject" `
  -noP4 -platform=HTML5 -clientconfig=Development `
  -build -cook -stage -pak -package -archive `
  -archivedirectory="C:\Path\HTML5-Archive" -utf8output
```

Require AutomationTool exit code 0. A successful archive normally contains a matching, current set of:

- the project `.html` launcher;
- JavaScript loader/glue;
- a WebAssembly module;
- packaged data and its loader;
- CSS, utility scripts, licenses, and launcher assets.

Compressed packages commonly use `.js.gz`, `.wasm.gz`, and `.data.gz`.

## Incremental safety

- Preserve prior packages in versioned directories.
- Do not delete `Intermediate`, `Saved`, or Derived Data Cache without a specific stale-artifact diagnosis.
- Reuse cooked content only if maps, assets, shader formats, and relevant settings are unchanged.
- Reuse a WebAssembly module only if no compiled C++ source, ABI choice, export set, or link option changed.
- Never substitute an older WebAssembly module merely because it has a convenient size or a previously successful startup path.

## Package verification

Compare file timestamps and verify that HTML, JavaScript, WebAssembly, and data belong to the same package run. Inspect the cook manifest for the intended map and dependencies. Then test over HTTP; `file://` is unsupported.
