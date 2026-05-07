# Firmware binaries

Always-current AR01 firmware for OTA updates.

## Files

- **`manifest.json`** — version metadata + checksums. Fetched first by the GUI.
- **`esp32-nexus.bin`** — ESP32-S3 firmware for the AR01 Nexus PCB. ~1.1 MB.
- **`stm32-app.bin`** — STM32F446RE application firmware (sectors 1-6). ~60 KB.

## Manifest schema

```jsonc
{
  "version": "v0.2.0",
  "released_at": "2026-05-06T12:34:56Z",
  "release_notes_url": "https://github.com/RehabtekLLC/AR01_FW/releases/tag/v0.2.0",
  "esp32": {
    "filename": "esp32-nexus.bin",
    "url": "https://raw.githubusercontent.com/RehabtekLLC/AR01_Updates/main/fw/esp32-nexus.bin",
    "sha256": "<hex>",
    "size": 1143344
  },
  "stm32": {
    "filename": "stm32-app.bin",
    "url": "https://raw.githubusercontent.com/RehabtekLLC/AR01_Updates/main/fw/stm32-app.bin",
    "sha256": "<hex>",
    "size": 56356
  }
}
```

## Notes

- The bootloader (`stm32-bootloader.bin`) is intentionally NOT mirrored here.
  It's flashed once via ST-Link and never updated OTA.
- The `dummy` ESP32 build is also not mirrored — it's a developer-only build.
