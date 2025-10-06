# gotdot-light-sensor-gpu

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
cmake -B gotdot-light-sensor-gpu-debug -DCMAKE_BUILD_TYPE=Debug .
cmake --build gotdot-light-sensor-gpu-debug --parallel 8

# Install
cmake --install gotdot-light-sensor-gpu-debug
```

## Usage in Godot

After install, copy the `gotdot-light-sensor-gpu` directory (containing the `.gdextension` and `lib/`) into your Godot project's `addons/` folder, then enable the plugin in Project Settings → Plugins. See `BUILD.md` → Integration with Godot.

## Contributing

Issues and pull requests are welcome. Please keep PRs focused and small.

## License

MIT © 2025 Jim Cortez. See `LICENSE.md` for details.
