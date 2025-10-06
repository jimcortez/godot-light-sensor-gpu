# Build Instructions

This document provides comprehensive instructions for building the `gotdot-light-sensor-gpu` Godot GDExtension project using CMake.

## Prerequisites

Before building this project, ensure you have the following installed:

- **[CMake](https://cmake.org/)** v3.22 or higher
- **C++ Compiler** with C++20 support:
  - **macOS**: Clang (comes with Xcode Command Line Tools)
  - **Linux**: GCC 11+ or Clang 12+
  - **Windows**: MSVC 2022 or later
- **Git** (for submodule management)
- **[ccache](https://ccache.dev/)** (optional, for faster rebuilds)
- **[clang-format](https://clang.llvm.org/docs/ClangFormat.html)** (optional, for code formatting)

## Initial Setup

### 1. Initialize Git Submodules

The project depends on the `godot-cpp` submodule. Initialize it first:

```bash
git submodule update --init --recursive
```


### 2. Verify Prerequisites

Check that you have the required tools:

```bash
cmake --version  # Should be 3.22+
c++ --version    # Should support C++20
```

## Using CMake Presets (optional)

This repository includes a `CMakePresets.json` with common configurations. You can use presets instead of passing flags manually.

List presets:

```bash
cmake --list-presets
```

Configure with a preset (examples):

```bash
# Debug
cmake --preset debug

# Release
cmake --preset release
```

Build and install using the preset build directory:

```bash
cmake --build gotdot-light-sensor-gpu-build --parallel 8
cmake --install gotdot-light-sensor-gpu-build
```

## Build and Install

### macOS Build (Universal - x86_64 + ARM64)

#### Debug Build (macOS)

```bash
# Create build directory and configure
cmake -B gotdot-light-sensor-gpu-debug \
      -DCMAKE_BUILD_TYPE=Debug \
      -DCMAKE_INSTALL_PREFIX=./gotdot-light-sensor-gpu-debug-install \
      .

# Build the project
cmake --build gotdot-light-sensor-gpu-debug --parallel 8

# Install the extension
cmake --install gotdot-light-sensor-gpu-debug
```

#### Release Build (macOS)

```bash
# Create build directory and configure
cmake -B gotdot-light-sensor-gpu-release \
      -DCMAKE_BUILD_TYPE=Release \
      -DCMAKE_INSTALL_PREFIX=./gotdot-light-sensor-gpu-release-install \
      .

# Build the project
cmake --build gotdot-light-sensor-gpu-release --parallel 8

# Install the extension
cmake --install gotdot-light-sensor-gpu-release
```

### Linux Build (x86_64)

#### Debug Build (Linux)

```bash
# Create build directory and configure
cmake -B gotdot-light-sensor-gpu-debug \
      -DCMAKE_BUILD_TYPE=Debug \
      -DCMAKE_INSTALL_PREFIX=./gotdot-light-sensor-gpu-debug-install \
      .

# Build the project
cmake --build gotdot-light-sensor-gpu-debug --parallel 8

# Install the extension
cmake --install gotdot-light-sensor-gpu-debug
```

#### Release Build (Linux)

```bash
# Create build directory and configure
cmake -B gotdot-light-sensor-gpu-release \
      -DCMAKE_BUILD_TYPE=Release \
      -DCMAKE_INSTALL_PREFIX=./gotdot-light-sensor-gpu-release-install \
      .

# Build the project
cmake --build gotdot-light-sensor-gpu-release --parallel 8

# Install the extension
cmake --install gotdot-light-sensor-gpu-release
```

### Windows Build (x86_64)

#### Debug Build (Windows)

```bash
# Create build directory and configure
cmake -B gotdot-light-sensor-gpu-debug \
      -G "Visual Studio 17 2022" \
      -A x64 \
      -DCMAKE_BUILD_TYPE=Debug \
      -DCMAKE_INSTALL_PREFIX=./gotdot-light-sensor-gpu-debug-install \
      .

# Build the project
cmake --build gotdot-light-sensor-gpu-debug --config Debug

# Install the extension
cmake --install gotdot-light-sensor-gpu-debug --config Debug
```

#### Release Build (Windows)

```bash
# Create build directory and configure
cmake -B gotdot-light-sensor-gpu-release \
      -G "Visual Studio 17 2022" \
      -A x64 \
      -DCMAKE_BUILD_TYPE=Release \
      -DCMAKE_INSTALL_PREFIX=./gotdot-light-sensor-gpu-release-install \
      .

# Build the project
cmake --build gotdot-light-sensor-gpu-release --config Release

# Install the extension
cmake --install gotdot-light-sensor-gpu-release --config Release
```

## Build Output

After a successful build and install, you'll find the following structure:

```text
gotdot-light-sensor-gpu-{debug|release}-install/
└── gotdot-light-sensor-gpu/
    ├── gotdot-light-sensor-gpu.gdextension
    ├── lib/
    │   └── {platform}-{architecture}/
    │       └── libgotdot-light-sensor-gpu.{so|dll|dylib}
    └── icons/
        └── Example.svg
```

### Platform-Specific Library Locations

- **macOS**: `lib/Darwin-universal/libgotdot-light-sensor-gpu.dylib`
- **Linux**: `lib/Linux-x86_64/libgotdot-light-sensor-gpu.so`
- **Windows**: `lib/Windows-x86_64/gotdot-light-sensor-gpu.dll`

## CMake Options

The project supports several CMake options for customization:

| Option | Description | Default |
|--------|-------------|---------|
| `CMAKE_BUILD_TYPE` | Build type (Debug/Release) | Debug |
| `CMAKE_INSTALL_PREFIX` | Installation directory | System default |
| `CCACHE_PROGRAM` | Path to ccache for faster rebuilds | Auto-detected |
| `CLANG_FORMAT_PROGRAM` | Path to clang-format for code formatting | Auto-detected |
| `GOTDOT_LIGHT_SENSOR_GPU_WARN_EVERYTHING` | Enable all compiler warnings | OFF |
| `GOTDOT_LIGHT_SENSOR_GPU_WARNING_AS_ERROR` | Treat warnings as errors | ON |

## Additional CMake Targets

The project provides several useful CMake targets:

- **`clang-format`**: Format all source code using clang-format
- **`install`**: Install the extension to the specified prefix

### Usage Examples

```bash
# Format code
cmake --build gotdot-light-sensor-gpu-debug --target clang-format

# Clean build
cmake --build gotdot-light-sensor-gpu-debug --target clean
```

## Troubleshooting

### Common Issues

1. **"godot-cpp submodule not found"**

   ```bash
   git submodule update --init --recursive
   ```

2. **"Cannot build in source directory"**
   - The project requires out-of-source builds
   - Use the `-B` flag to specify a build directory

3. **"C++20 not supported"**
   - Update your compiler to a version that supports C++20
   - On macOS: Update Xcode Command Line Tools
   - On Linux: Install GCC 11+ or Clang 12+

4. **Build fails on Windows**
   - Ensure you have Visual Studio 2022 installed
   - Use the correct generator: `-G "Visual Studio 17 2022"`
   - Specify the architecture: `-A x64`

### Performance Tips

- **IMPORTANT**: Always use `--parallel 8` (or lower) flag with `cmake --build` for faster compilation. Without limiting parallel jobs, the build will consume all CPU cores and may crash your system due to resource exhaustion.
- Install `ccache` for faster rebuilds
- Use `CMAKE_BUILD_TYPE=Release` for optimized builds

## Integration with Godot

To use the built extension in your Godot project:

1. Copy the entire `gotdot-light-sensor-gpu` directory from your install location to your Godot project's `addons/` folder
2. Enable the extension in Godot's Project Settings → Plugins
3. The extension classes will be available in your GDScript code

## Development Workflow

For development, it's recommended to:

1. Use Debug builds for development and testing
2. Use Release builds for performance testing and distribution
3. Run `clang-format` regularly to maintain code style
4. Clean build directories when switching between Debug/Release builds

```bash
# Clean previous builds
rm -rf gotdot-light-sensor-gpu-debug gotdot-light-sensor-gpu-release

# Rebuild from scratch
cmake -B gotdot-light-sensor-gpu-debug -DCMAKE_BUILD_TYPE=Debug .
cmake --build gotdot-light-sensor-gpu-debug --parallel 8
```