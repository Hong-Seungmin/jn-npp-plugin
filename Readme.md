# jN Npp Plugin

> This repository is a maintained fork of [sieukrem/jn-npp-plugin](https://github.com/sieukrem/jn-npp-plugin).
> Issues and pull requests for this fork are welcome here; fixes that are useful upstream are offered back as pull requests.

`jN Npp Plugin` is a plugin for Notepad++, which allows you to extend Notepad++ by writing JavaScript code.

## Technology

`jN` uses the built-in javascript engine of Microsoft Windows. This powerful engine allows to access a lot of ActiveX based
services like Shell, WMI of operating system.

`jN` wraps the native Notepad++ API into ActiveX interfaces accessible via global objects `Editor` and `System` in your JavaScript code.

## How to Use - Getting Started

You will find the feature list and examples in [wiki](https://github.com/sieukrem/jn-npp-plugin/wiki).

## For Developers

### Folder Structure

- `common` - implementation of Notepad++ independent ActiveX elements (e.g. Dialog, Menu, WinApi, System, ...).
- `editor` - implementation of Notepad++ related ActiveX elements (e.g. DockableDialog, View, ViewLine).
- `npp` - copy-in files from original Notepad++ plugin template project.
- `deploy` - collection of JavaScript files, which were meant to show capabilities of `jN`, but contain also some useful functions like XML, Grep, Zen Coding, SmartHighlighter. 

### Building

Prerequisites:

- Visual Studio 2022 or later with the **Desktop development with C++** workload
  (MSVC v143 toolset is the project default; any newer toolset works with `-p:PlatformToolset=v14x`).
- A **Windows 10/11 SDK** (provides the headers, libraries and `midl.exe` used to compile the type library).

Build from Visual Studio by opening `jN.sln`, or from a Developer Command Prompt:

```
msbuild jN.vcxproj -m -p:Configuration=Release -p:Platform=x64
msbuild jN.vcxproj -m -p:Configuration=Release -p:Platform=Win32
```

The version embedded into `jN.dll` comes from the `VersionMajor`, `VersionMinor`, `BuildNumber` and `VersionRevision`
MSBuild properties. CI derives them with [Nerdbank.GitVersioning](https://github.com/dotnet/Nerdbank.GitVersioning)
(`version.json`); pass them explicitly for a local build, e.g. `-p:VersionMajor=2 -p:VersionMinor=2 -p:BuildNumber=191`.

> Rebuild entire solution every time you modified any of `*.idl` files! MIDL regenerates `Interfaces.h` and
> `project.tlb`, and the type library is embedded into the DLL through `res.rc`.

### Installing a local build

Copy `jN.dll` together with the `deploy/jN` folder into `<Notepad++>\plugins\jN\` so that the layout is
`plugins\jN\jN.dll` and `plugins\jN\jN\start.js`, then restart Notepad++.

### Continuous Integration

- `.github/workflows/build.yml` builds x86/x64 × Debug/Release on every push and pull request and uploads the DLLs and zips as artifacts.
- `.github/workflows/create-release.yml` (manual trigger) builds the Release configurations, tags the commit with the
  Nerdbank.GitVersioning version and creates a draft GitHub release with `jN_<version>_x64.zip` and `jN_<version>_x86.zip`.
