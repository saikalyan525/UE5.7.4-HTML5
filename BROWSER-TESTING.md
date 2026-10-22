# Browser Hosting and Testing

Never open a packaged build using `file://`. Serve it from HTTP(S) with explicit MIME and compression headers.

## Browser requirements

JSPI builds require a Chromium-family browser that supports:

- WebAssembly Memory64;
- JavaScript Promise Integration;
- `WebAssembly.Suspending`;
- `WebAssembly.promising`;
- WebGL 2.

Browser support changes. Record the exact browser version used for every result.

## Required response headers

For precompressed package files:

| File | Content-Type | Content-Encoding |
| --- | --- | --- |
| `.wasm.gz` | `application/wasm` | `gzip` |
| `.js.gz` | `application/javascript` | `gzip` |
| `.data.gz` | `application/octet-stream` | `gzip` |
| `.symbols.gz` | `application/octet-stream` | `gzip` |

Serve HTML as `text/html`, CSS as `text/css`, and uncompressed JavaScript as `application/javascript`. Do not let a CDN compress an already compressed payload again.

If a build enables pthreads or `SharedArrayBuffer`, it normally also needs cross-origin isolation:

```text
Cross-Origin-Opener-Policy: same-origin
Cross-Origin-Embedder-Policy: require-corp
```

Every subresource must satisfy the corresponding CORS/CORP rules. GitHub Pages does not provide arbitrary response-header control and may therefore be unsuitable for some builds.

## Validation sequence

HTTP 200 and an HTML canvas are not gameplay proof. Validate in this order:

1. HTML and loader JavaScript execute.
2. Data downloads and the virtual filesystem mounts.
3. WebAssembly downloads, compiles, and instantiates.
4. Static constructors complete and `main` is reached.
5. Unreal allocator, task, file, and logging systems initialize.
6. WebGL 2 and the RHI initialize.
7. Shader libraries load without missing permutations.
8. The configured startup map loads.
9. The first visible frame renders.
10. Rendering/ticking continues for at least 60 seconds.
11. Keyboard, mouse, pointer lock, UI, and audio work as applicable.
12. A cache-disabled reload repeats the result.

Capture the first JavaScript or WebAssembly exception and its complete stack before changing engine code. A black canvas may be a rendered black frame, a startup assertion, an infinite native loop, an unsupported shader path, or a terminated renderer.

## Result language

Report each milestone separately as PASS, FAIL, or NOT TESTED. Packaging success must not be described as browser success, and map loading must not be described as visible gameplay.
