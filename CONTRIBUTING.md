# Contributing

Thank you for helping improve the UE 5.7.4 HTML5 experiment.

## Repository boundary

This public repository accepts documentation, reproducibility reports, and links to legally redistributable tooling. Do not submit:

- Unreal Engine source or source excerpts that Epic does not permit publicly;
- Epic dependency downloads or generated engine binaries;
- Marketplace/Fab or proprietary project assets;
- installed SDKs, cooked content, packaged games, caches, or credentials;
- proprietary Epic-bundled components such as Oodle.

Engine-source changes belong in the licensed fork and are visible only to users authorized for EpicGames/UnrealEngine. Do not open a public issue containing restricted source.

## High-value reports

A useful reproducibility report includes:

- exact licensed branch commit;
- Windows, Visual Studio, MSVC, and Windows SDK versions;
- Git, Git Bash, CMake, Python, Node, Ninja, and Perl versions;
- exact Emscripten version (`4.0.12` is required by the current branch);
- the complete command and exit code;
- the first real error, not only the final AutomationTool summary;
- whether the failure is editor build, HTML5 compile, link, cook, stage, package, HTTP delivery, WebAssembly startup, renderer initialization, map load, first frame, ticking, or input;
- browser name/version, console stack, and relevant response headers;
- confirmation that no private source, credentials, or proprietary assets are attached.

## Testing discipline

- Preserve successful intermediates and versioned packages.
- Do not clear all caches without evidence.
- Do not infer progress from CPU or RAM alone.
- Do not report success until the exact stated milestone is observed.
- Stop at the first new confirmed failure before applying speculative engine changes.

## Documentation changes

Keep claims conservative and evidence-backed. Mark unverified commands or platforms clearly. Links to the licensed fork must remind readers that Epic authorization is required.

By contributing, you confirm that you have the right to submit the material and that it does not expose licensed Unreal Engine source or third-party proprietary content.
