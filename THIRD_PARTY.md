# Third-Party Projects

This project stands on the shoulders of several existing open source efforts. We do **not** duplicate their work; instead we build tooling and documentation on top of them.

## 1. Boot & hardware experimentation

### AsahiLinux/m1n1

- **Repo**: https://github.com/AsahiLinux/m1n1  
- **Use for**:
  - Understanding Apple Silicon boot flow and how to insert a “stage-2” shim.
  - Patterns for device-tree handling, MMIO probing, and hardware bring-up.
  - Examples of RTKit/ASC communication and logging.
- **How we depend on it**:
  - We treat `m1n1` as a design and code reference.
  - Where licensing permits, we may adapt small self-contained components (e.g. ADT parsers), clearly attributed.
  - We do **not** ship Apple firmware or macOS binaries; we only ship our own code and docs.

## 2. Firmware, IPSW, and Image4 tooling

### `ipsw` (blacktop/ipsw)

- **Docs**: https://blacktop.github.io/ipsw/docs/cli/ipsw/  
- **Use for**:
  - Extracting and analyzing `.ipsw` images.
  - Getting kernelcaches, DeviceTrees, dyld shared caches, and IMG4 payloads.
- **How we depend on it**:
  - Our extraction scripts call `ipsw` as a CLI (or via library bindings where convenient).
  - We focus on higher-level orchestration, status tracking, and documentation, not low-level parsing.

### `img4tool` (tihmstar/img4tool)

- **Repo**: https://github.com/tihmstar/img4tool  
- **Use for**:
  - Inspecting and manipulating IMG4/IM4P/IM4M files.
- **How we depend on it**:
  - We wrap `img4tool` for tasks like unpacking specific images (kernelcache, DeviceTree).
  - Its output is consumed by our own tools for analysis and visualization.

## 3. Firmware extraction model

### Asahi `asahi-fwextract` and installer scripts

- **Example**: https://github.com/UbuntuAsahi/asahi-fwextract  
- **Use for**:
  - The overall pattern of extracting vendor firmware from the user’s macOS and packing it into a `vendorfw.cpio` archive that is loaded at boot.
- **How we depend on it**:
  - We adopt the same **model**:
    - Provide an open-source extractor (e.g. `iphone-fwextract`) that runs on the user’s machine.
    - Use `ipsw` / `img4tool` under the hood to locate necessary firmware inside iOS/macOS.
    - Package it into a “vendor firmware” bundle that our boot flow and/or OS can consume.
  - We do **not** redistribute Apple firmware; users extract it from their own legally obtained OS images.

## 4. GPU & display stack

### Mesa Asahi driver (`asahi`)

- **Docs**: https://docs.mesa3d.org/drivers/asahi.html  
- **Use for**:
  - Understanding Apple AGX GPU architecture, command stream formats, and shader compiler design.
  - Reference implementation of an open-source userspace driver talking to Apple GPU firmware.
- **How we depend on it**:
  - We treat the Mesa Asahi code as a reference for any GPU-related work on iPhone-class hardware.
  - Where hardware is similar, we may adapt or reuse portions of the shader compiler and driver logic, subject to licensing.
  - Our focus is on mapping those ideas to iPhone-specific GPUs and constraints.

## 5. General iOS/macOS reverse-engineering tools

Besides the specific projects above, we rely on the broader iOS/macOS RE ecosystem, including but not limited to:

- `ipsw` (again) for IPSW parsing and extraction.
- `img4tool` and related Image4 utilities.
- Mach-O, dyld shared cache, and device-tree tooling from existing communities.

We do **not** aim to replace these tools. Instead, this repo provides:

- Higher-level orchestration and automation.
- Documentation that ties raw tool output back to our macOS-on-iPhone goals.
- A clear status matrix and roadmap for bringing macOS-style environments to iPhone-class hardware.

