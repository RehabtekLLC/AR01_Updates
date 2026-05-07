# AR01 Updates

Public distribution channel for AR01 firmware binaries and GUI installers.
Used by the AR01 GUI app to check for and download updates.

## Layout

```
AR01_Updates/
├── fw/                    Firmware binaries + manifest
│   ├── manifest.json      Current version + URLs + checksums
│   ├── esp32-nexus.bin    Latest ESP32 firmware (always overwritten)
│   └── stm32-app.bin      Latest STM32 firmware (always overwritten)
└── gui/                   GUI installers (populated by AR01 GUI repo)
```

Source code for these artifacts lives in private repos
(`RehabtekLLC/AR01_FW`, `RehabtekLLC/AR01_GUI`). This repo only carries
build outputs.

## Firmware update check (for GUI developers)

The GUI fetches firmware metadata from a stable raw URL — no GitHub API
auth needed:

```
https://raw.githubusercontent.com/RehabtekLLC/AR01_Updates/main/fw/manifest.json
```

The manifest declares the current version, download URLs, and SHA-256
checksums for `esp32-nexus.bin` and `stm32-app.bin`. The GUI compares
this version with the device's BLE `FwVersion` characteristic
(`a1010008`) and offers an in-app update via the BLE OTA characteristic.

Historical versions are also published as tagged GitHub Releases on this
repo (matching the `AR01_FW` tag, e.g. `v0.2.0`), with the same
binaries attached as release assets.

## Firmware publishing flow

When `AR01_FW` pushes a `v*.*.*` tag, its release CI:

1. Builds bootloader + app + ESP32 firmware (stamped with the tag).
2. Publishes a release on the private `AR01_FW` repo.
3. **Mirrors `esp32-nexus.bin` + `stm32-app.bin` to this repo**:
   - Commits new files to `fw/` on `main` (for the always-latest path).
   - Creates a matching tagged release on this repo (for history + API).
4. Updates `fw/manifest.json` with the new version + checksums.

No manual upload step.
