# Cortex ARM Dev Container Workspace

This repo is the shared workspace/devcontainer host for multiple C/C++ projects.
Project source repos should stay separate and live next to this repo, not inside it.

## Recommended layout

Keep project repos as siblings of this repo:

```text
<parent-folder>/
  cortex_devcontainer_workspace/   # shared devcontainer + workspace config
  cortexm-hello/                   # project repo
  another-fw-project/              # project repo
  bootloader-project/              # project repo
```

This workspace uses the Docker image defined in:

- `cortex_cross/clang_arm_cortex_dockerfile`

## Open in Dev Container

1. Open this folder in VS Code:
   - `cortex_devcontainer_workspace`
2. Run **Dev Containers: Reopen in Container**.
3. Open the multi-root workspace file:
  - `workspace.code-workspace`

The devcontainer mounts the parent folder at `/workspace`, so sibling repos are visible in one container session.

The container provides:

- Clang 18
- `arm-none-eabi-gcc` cross compiler
- CMake built from source
- ARM runtime toolchain files at `/opt/toolchains`

## Toolchain files

Inside the container:

- `/opt/toolchains/arm-none-eabi-gcc.cmake`
- `/opt/toolchains/arm-none-eabi-clang.cmake`

Example configure command:

```bash
cmake -S . -B build-cortex \
  -G Ninja \
  -DCMAKE_TOOLCHAIN_FILE=/opt/toolchains/arm-none-eabi-clang.cmake
```

## Project conventions

- Keep build artifacts inside each project repo (`build/`, `build-clang/`, etc.).
- Default to shared ARM toolchains from `/opt/toolchains`.
- Allow per-project overrides in each project's `CMakeLists.txt` or local build presets.
