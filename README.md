# GlowEngine

A custom C++ 3D game engine for Windows, built on DirectX 11. It uses an entity–component–system (ECS) layout, a real-time forward-style rendering pipeline (including shadow mapping), integrated ImGui tooling, and gameplay-oriented systems such as physics and audio.

## Features

| Area | Details |
|------|---------|
| Rendering | DirectX 11 renderer, HLSL vertex/pixel shaders (including shadow map passes), materials, mesh library, texture loading (including `stb_image`), lighting buffers, shadow system |
| Scene & data | Scene system with sample content (e.g. forest scene), JSON-driven entity definitions under `GlowEngine/Data/Entities/`, Assimp-based model loading |
| Gameplay | ECS-style entities and components, transforms, physics, box colliders and collision resolution, player behavior |
| Editor / UI | In-engine editor UI (`GlowGui`): game window, scene editor, inspector, console, resources, settings |
| Audio | FMOD-backed sound system and sound library |
| Input | Keyboard and mouse input system; camera and player controls in the demo |

## Tech stack

- Language: C++17 (`stdcpp17` on x64 configurations)
- Graphics: DirectX 11, HLSL (`Source/Shaders/`)
- UI: Dear ImGui (Win32 + DX11 backends)
- Models: [Assimp](https://www.assimp.org/) (linked as `assimp-vc143-mt.lib`)
- Audio: [FMOD](https://www.fmod.com/) (linked as `fmodL_vc.lib` on x64)
- Other: JSON parsing for entities ([nlohmann/json](https://github.com/nlohmann/json) single-header in-tree), precompiled headers on main engine units

## Requirements

- OS: Windows 10 or later (project targets Windows SDK `10.0`)
- IDE: Visual Studio 2022 (toolset v143; solution format VS 2022)
- Workload: Desktop development with C++, including the Windows 10/11 SDK and MSVC toolset
- Optional: Windows SDK version matching `WindowsTargetPlatformVersion` in `GlowEngine.vcxproj` if you hit SDK-related build errors

Third-party import libraries are expected under `GlowEngine/Lib/` (e.g. Assimp and FMOD). If those folders are empty in your clone, obtain matching v143 / x64 builds and place `.lib` files (and any required runtime DLLs next to the built `.exe`) before running.

## Building

1. Open `GlowEngine.sln` at the repository root in Visual Studio.
2. Select Debug | x64 or Release | x64 (recommended; full include paths and FMOD are wired for x64 in the project file).
3. Build the GlowEngine project (Build → Build Solution or `Ctrl+Shift+B`).

The Pre-Link step runs `run.bat` from the solution directory, which copies `GlowEngine/Assets` into `x64/Debug/Assets/` so the executable can find content when run from the usual output folder.

Typical output:

- Debug: `x64/Debug/GlowEngine.exe` (Debug x64 uses the console subsystem for easier logging)
- Release: `x64/Release/GlowEngine.exe` (Release x64 uses the Windows subsystem)

## Running the demo

After a successful build, start `GlowEngine.exe` from the configuration output folder (e.g. `x64/Debug`). The demo exercises the ECS, rendering, physics, editor UI, and camera/player movement.

### Controls

| Input | Action |
|--------|--------|
| W A S D | Move camera |
| Space | Move up |
| Shift | Move down |
| Mouse | Look |
| Right-click | Pivot in-scene |
| Tab | Toggle fullscreen |
| G | Toggle debug visuals |

## Repository layout (high level)

```
GlowEngine.sln          # Visual Studio solution
run.bat                 # Pre-link: copies Assets → x64/Debug/Assets
GlowEngine/
  Assets/               # Meshes, textures, etc.
  Data/Entities/        # JSON entity descriptions
  Include/              # Third-party headers (e.g. Assimp, FMOD)
  Lib/                  # Third-party .lib files (populate locally if missing)
  Source/
    Engine/             # Core engine: graphics, ECS, audio, input, …
    Game/               # Game layer: scenes, behaviors, systems
    Shaders/            # HLSL shaders
```

## Credits

Callen Betts

---

Educational / portfolio engine; APIs and third-party terms apply to Assimp, FMOD, ImGui, and Microsoft SDKs.
