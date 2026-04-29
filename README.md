# UltralightNet.Bundle

A single-package .NET binding for the [Ultralight](https://ultralig.ht/) HTML rendering engine.

This package bundles everything needed to embed Ultralight in a .NET application:

- `UltralightNet` - managed bindings for the Ultralight C API
- `UltralightNet.AppCore` - managed bindings for AppCore (windowing, font loader, file system, default logger)
- Native binaries for `win-x64`, `linux-x64`, and `osx-x64` (Ultralight, UltralightCore, WebCore, AppCore)

It targets **.NET 10**.

## Install

```sh
dotnet add package UltralightNet.Bundle
```

## Quick start

```csharp
using UltralightNet;
using UltralightNet.AppCore;

AppCoreMethods.SetPlatformFontLoader();
AppCoreMethods.ulEnablePlatformFileSystem("./assets");
AppCoreMethods.ulEnableDefaultLogger("./ultralight.log");

using var renderer = ULPlatform.CreateRenderer(new ULConfig());
using var view = renderer.CreateView(1280, 720, new ULViewConfig());

view.URL = "https://example.com";

while (!view.IsLoading)
{
    renderer.Update();
    renderer.Render();
}
```

The native binaries are deployed automatically into `runtimes/<rid>/native/` when your project builds.

## Supported runtimes

| RID         | Libraries |
|-------------|-----------|
| `win-x64`   | `Ultralight.dll`, `UltralightCore.dll`, `WebCore.dll`, `AppCore.dll` |
| `linux-x64` | `libUltralight.so`, `libUltralightCore.so`, `libWebCore.so`, `libAppCore.so` |
| `osx-x64`   | `libUltralight.dylib`, `libUltralightCore.dylib`, `libWebCore.dylib`, `libAppCore.dylib` |

## Build & pack locally

```sh
dotnet pack -c Release
```

Output: `bin/Release/UltralightNet.Bundle.<version>.nupkg` (and `.snupkg`).

## License

Source code in this repository is licensed under [MPL-2.0](LICENSE).

The bundled Ultralight native binaries are redistributed under the
[Ultralight Free License](https://ultralig.ht/) - review their terms before
shipping a commercial product.

