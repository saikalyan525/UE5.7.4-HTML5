# Installation

This guide describes access and setup for authorized Unreal Engine source developers. Clean-install reproducibility is still under validation, so record the first failing command and its complete output when reporting a problem.

## 1. Obtain authorized source access

You need:

- An Epic Games account linked to GitHub.
- Access to the private `EpicGames/UnrealEngine` network.
- Permission to view [saikalyan525/UnrealEngine](https://github.com/saikalyan525/UnrealEngine/tree/UE5.7.4-HTML5).

The fork may return 404 until Epic's GitHub authorization is active. This documentation repository cannot grant engine-source access.

## 2. Host prerequisites

The currently documented host environment is:

- Windows 10 or later.
- Visual Studio 2022 with Desktop development with C++ and Game development with C++.
- A Windows SDK compatible with Unreal Engine 5.7.4.
- Git and Git LFS.
- Git Bash for the HTML5 shell scripts.
- CMake, Perl, and Ninja where required by Unreal's HTML5 third-party builds.
- Sufficient free storage for Unreal dependencies, engine objects, SDKs, cooked data, and versioned packages.

Use the Python and Node versions installed/activated by emsdk for Emscripten operations.

## 3. Clone the licensed branch

```powershell
git clone --branch UE5.7.4-HTML5 https://github.com/saikalyan525/UnrealEngine.git UE574-HTML5
Set-Location UE574-HTML5
git rev-parse HEAD
```

Confirm that the branch descends from Epic's `5.7.4-release` commit:

```text
260bb2e1c5610b31c63a36206eedd289409c5f11
```

## 4. Download Epic dependencies

```powershell
.\Setup.bat
.\GenerateProjectFiles.bat
```

`Setup.bat` may request administrator approval for Microsoft's Visual C++ Redistributable. Allow the existing setup process to finish rather than starting another copy.

Do not commit or redistribute dependency downloads or generated engine binaries.

## 5. Install the HTML5 SDK

From Git Bash:

```bash
cd Engine/Platforms/HTML5
./HTML5Setup.sh
```

The intended setup installs and activates Emscripten `4.0.12`, applies the compatibility overlay tracked by the licensed branch, and builds the HTML5 third-party libraries. A plain upstream Emscripten installation without the branch overlay and matching libraries is not equivalent.

The installed SDK, generated libraries, intermediate source copies, caches, and logs must remain uncommitted.

## 6. Verify the SDK

In the activated Emscripten environment:

```powershell
emcc --version
em++ --version
node --version
python --version
```

Both compiler commands must report Emscripten `4.0.12`.

Run Unreal's SDK check and require HTML5 to report `Status=Valid` and `Current_Sdk=4.0.12` before compiling:

```powershell
Engine\Build\BatchFiles\RunUAT.bat Turnkey -command=VerifySdk -platform=HTML5 -utf8output
```

## Reproducibility warning

The independent clean-checkout validation is not complete. A successful SDK version check is only a prerequisite; it does not prove that engine compilation, project cooking, packaging, or browser gameplay will succeed.
