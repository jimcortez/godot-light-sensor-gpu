# godot-light-sensor-gpu

> **⚠️ WARNING: This project is not useful yet and is under active development.**
>
> This is an experimental project in early development stages. The functionality is incomplete, unstable, and not ready for production use. Use at your own risk.

A light sensor node that allows for efficient measurement of light values in 3d spaced, optimized for GPU throughput.

## Overview

This repository contains a Godot 4 GDExtension implemented in C++ (using CMake) that provides a high-performance light sensor node designed to sample lighting values in 3D scenes with GPU-friendly throughput.

- Godot version: 4.x (see your Godot project compatibility)
- Language: C++20
- Build system: CMake
- License: MIT (see `LICENSE.md`)

## Getting Started

- Installation and full build instructions are in `BUILD.md`.
- Pre-requisites and platform-specific notes are also documented there.

Quick start (see more in `BUILD.md`):

```bash
# Initialize submodules
git submodule update --init --recursive

# Configure and build (Debug example)
cmake -B godot-light-sensor-gpu-debug -DCMAKE_BUILD_TYPE=Debug .
cmake --build godot-light-sensor-gpu-debug --parallel 8

# Install
cmake --install godot-light-sensor-gpu-debug
```

## Add this plugin to a Godot project

You can either use prebuilt artifacts (from your local install step or a release) or build locally. In both cases, your Godot project should end up with this structure:

```text
res://addons/godot-light-sensor-gpu/
├── godot-light-sensor-gpu.gdextension
├── lib/
│   └── {platform}-{arch}/
│       └── libgodot-light-sensor-gpu.{dylib|so|dll}
└── icons/
    └── Example.svg
```

### Option A: Use prebuilt artifacts

1. Obtain the `godot-light-sensor-gpu` folder produced by install (see `BUILD.md` → Build Output) or download it from a release.
2. Copy that entire folder into your Godot project at `res://addons/godot-light-sensor-gpu/`.
3. Make sure the `.gdextension` file and the `lib/` subfolder (with your platform binary) are present.
4. Open or restart your Godot project. If this extension ships an editor plugin, enable it in Project Settings → Plugins. Otherwise, the registered classes will be available automatically in the Add Node dialog and from scripts.

### Option B: Build locally and install

1. Follow the steps in `BUILD.md` (use a Debug or Release build as needed).
2. After `cmake --install`, locate the install output at `godot-light-sensor-gpu-{debug|release}-install/godot-light-sensor-gpu/`.
3. Copy that `godot-light-sensor-gpu` directory into your Godot project at `res://addons/`.
4. Restart Godot and (if applicable) enable the plugin in Project Settings → Plugins.

### Option C: Development (submodule inside a Godot project)

Develop the addon and your game/project side-by-side using a Git submodule and installing directly into the project's `addons/` folder.

1. In your Godot project root, add this repository as a submodule:

   ```bash
   cd /path/to/YourGodotProject
   git submodule add https://github.com/jecortez/godot-light-sensor-gpu third_party/godot-light-sensor-gpu
   git submodule update --init --recursive
   ```

2. Configure a dedicated build directory inside your Godot project and set the install prefix to your project's addons path:

   ```bash
   # From your Godot project root
   cmake -B third_party/godot-light-sensor-gpu-build \
         -S third_party/godot-light-sensor-gpu \
         -DCMAKE_BUILD_TYPE=Debug \
         -DCMAKE_INSTALL_PREFIX="$PWD/addons/godot-light-sensor-gpu"

   cmake --build third_party/godot-light-sensor-gpu-build --parallel 8
   cmake --install third_party/godot-light-sensor-gpu-build
   ```

3. In Godot, enable the plugin in Project Settings → Plugins (if applicable). Godot may hot-reload native libraries after rebuilds; if not, restart the editor or the running scene.

4. Iterate quickly during development by rebuilding and re-installing into the project:

   ```bash
   cmake --build third_party/godot-light-sensor-gpu-build --parallel 8 \
     && cmake --install third_party/godot-light-sensor-gpu-build
   ```

Notes for development:

- The `.gdextension` file in `addons/godot-light-sensor-gpu/` references binaries in `lib/{platform}-{arch}/`. Keep that structure intact.
- The editor loads the platform-matching binary (e.g., macOS loads `Darwin-universal`). To test exports for other platforms, build and place the corresponding binaries under `lib/` as well.
- If you use CMake presets, you can pass `-S third_party/godot-light-sensor-gpu -B third_party/godot-light-sensor-gpu-build -DCMAKE_INSTALL_PREFIX="$PWD/addons/godot-light-sensor-gpu"` to keep the same workflow.

Notes:

- Keep the folder name and relative structure intact; the `.gdextension` file references libraries by relative paths inside `lib/`.
- If you plan to export to multiple platforms, include the appropriate binaries under `lib/{platform}-{arch}/` for each target.
- For more detail, see `BUILD.md` → Integration with Godot.

## Contributing

Issues and pull requests are welcome. Please keep PRs focused and small.

## License

MIT © 2025 Jim Cortez. See `LICENSE.md` for details.
