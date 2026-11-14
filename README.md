# Box3D

[![Build Status](https://github.com/waskosky/box3d/actions/workflows/build.yml/badge.svg)](https://github.com/waskosky/box3d/actions)

![Box3D Logo](https://box2d.org/images/logo.svg)

Box3D is a 3D physics engine for games.

## Maintained downstream fork

This is [waskosky/box3d](https://github.com/waskosky/box3d), a maintenance fork of
[Erin Catto's Box3D](https://github.com/erincatto/box3d). Upstream remains the
original project; its authorship and MIT license are unchanged.

We maintain tested native fixes here without waiting for upstream acceptance.
`main` is our baseline; consumers should pin a tested full commit from this fork.
Upstream updates are reviewed and integrated while preserving our fixes and tests.
This is independent of any application or engine adapter.

The first downstream correction fixes the CCD impact-to-response handoff for
small, fast bodies against thin hulls, meshes and heightfields. It is native source,
not a consumer-side patch script, and is covered by `CCDResponseTest` (774 cases).
Impact timing and deterministic trajectory goldens differ from uncorrected
upstream builds; old recordings/replays are not promised to match. A corrected
source build does not update previously embedded binaries or export templates.

Build the native tests with samples disabled:

```sh
cmake -S . -B build -DCMAKE_BUILD_TYPE=Debug -DBOX3D_SAMPLES=OFF
cmake --build build --parallel
./build/bin/test CCDResponseTest
./build/bin/test
```

For multi-configuration generators, use the appropriate configuration subdirectory.
The documentation below describes the shared Box3D API and upstream project.

[![Introducing Box3D](https://img.youtube.com/vi/jr_Fzl2XwKU/maxresdefault.jpg)](https://www.youtube.com/watch?v=jr_Fzl2XwKU)

## Features

### Collision

- Continuous collision detection
- Contact events
- Convex hulls, capsules, spheres, triangle meshes, and height fields
- Multiple shapes per body
- Collision filtering
- Ray casts, shape casts, and overlap queries
- Sensor system
- Character mover

### Physics

- Robust _Soft Step_ rigid body solver
- Continuous physics for fast translations and rotations
- Island based sleep
- Revolute, prismatic, distance, motor, weld, and wheel joints
- Joint limits, motors, springs, and friction
- Joint and contact forces
- Body movement events and sleep notification

### System

- Data-oriented design
- Written in portable C17
- Extensive multithreading and SIMD
- Optimized for large piles of bodies
- Cross platform determinism
- Recording and replay

### Samples

- Uses sokol to run with D3D11 on Windows, Metal on macOS, and OpenGL 4.5 on Linux.
- Graphical user interface with imgui.
- Many samples to demonstrate features and performance.

## Building all platforms

- Install [CMake](https://cmake.org/)
- Install [git](https://git-scm.com/)
- Ensure these run from the command line

## Building with CMake presets (recommended)

This uses the presets in `CMakePresets.json`.

- Windows: `cmake --preset windows` then `cmake --build --preset windows-release`
- Linux: `cmake --preset linux-release` then `cmake --build --preset linux-release`
- macOS: `cmake --preset macos` then `cmake --build --preset macos-release`
- Windows MinGW: `cmake --preset mingw-release` then `cmake --build --preset mingw-release`

Run the samples app (must be in the Box3D directory).

- Windows: `.\build\bin\Release\samples.exe`
- Linux: `./build/bin/samples`
- macOS: `./build/bin/Release/samples`

## Building for Visual Studio

- Install [Visual Studio](https://visualstudio.microsoft.com/)
- Run `build_vs2026.bat`
- Open and build `build/box3d.slnx`

## Building for Linux

- Run `build.sh` from a bash shell
- Results are in the build sub-folder

## Building for Xcode

- mkdir build
- cd build
- cmake -G Xcode ..
- Open `box3d.xcodeproj`
- Select the samples scheme
- Build and run the samples

## Building for Web

- [Emscripten SDK](https://emscripten.org/docs/getting_started/downloads.html)
- `emcmake cmake -B build -DBOX3D_SAMPLES=OFF`
- `cmake --build build`

Box3D uses SSE2 with WebAssembly. Define `BOX3D_DISABLE_SIMD` to disable SSE2.

## Building and installing

- mkdir build
- cd build
- cmake ..
- cmake --build . --config Release
- cmake --install . (might need sudo)

## Using Box3D in your project

The core library has no dependencies beyond the C runtime (and `libm` on Unix). Linking it
gives you the `box3d::box3d` target.

I recommend to use FetchContent:

```cmake
include(FetchContent)
FetchContent_Declare(box3d
      GIT_REPOSITORY https://github.com/waskosky/box3d.git
      GIT_TAG main) # Pin a tested full commit for reproducible consumer builds.
FetchContent_MakeAvailable(box3d)

target_link_libraries(my_app PRIVATE box3d::box3d)
```

For a vendored copy or git submodule, point `add_subdirectory` at it:

```cmake
add_subdirectory(extern/box3d)

target_link_libraries(my_app PRIVATE box3d::box3d)
```

To use a copy installed with `cmake --install`, find the package:

```cmake
find_package(box3d 0.1 REQUIRED)

target_link_libraries(my_app PRIVATE box3d::box3d)
```

See [`docs/hello.md`](docs/hello.md) for a minimal first program.

## Compatibility

The Box3D library and samples build and run on Windows, Linux, and Mac.

You will need a compiler that supports C17 to build the Box3D library.

You will need a compiler that supports C++20 to build the samples.

Box3D uses SSE2, AVX2, and Neon (AArch64) SIMD math to improve performance. This can be disabled by
defining `BOX3D_DISABLE_SIMD`. SSE2 is automatically used as a fallback when the CPU doesn't support
AVX2.

## Documentation

The user manual lives in [`docs/`](docs/) and is built with Doxygen. Enable the `BOX3D_DOCS` CMake option and build the `doc` target.

## Community

- [Discord](https://discord.gg/NKYgCBP)

## Contributing

For this fork, see [CONTRIBUTING.md](CONTRIBUTING.md). Native fixes can be
maintained here independently. Upstream currently requests an issue/discussion
before any invited contribution; that process is separate from our releases.

## Giving feedback

Please file an issue or start a chat on discord. You can also use [GitHub Discussions](https://github.com/erincatto/box3d/discussions).

## License

Box3D is developed by Erin Catto and uses the [MIT license](https://en.wikipedia.org/wiki/MIT_License).

## Sponsorship

Support development of Box3D through [Github Sponsors](https://github.com/sponsors/erincatto).

Please consider starring the original project and subscribing to
[Erin Catto's YouTube channel](https://www.youtube.com/@erin_catto).

## LLM Usage

This fork includes AI-assisted changes and tests. They must be reviewed and
validated like any other change; assistance does not replace physics regression
coverage or qualification. Erin Catto's upstream authorship and
[LLM policy](https://github.com/erincatto/box3d#llm-usage) apply to the original
project, not to authorship of this fork's additional changes.
