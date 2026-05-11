# AR01 Updates

Public artifact hub for the AR01 platform. Hosts firmware binaries +
manifest under `fw/`, GUI installer manifest under `gui/`, and the
matching GitHub Releases that serve those artifacts. Used by both the
device firmware update flow and the AR01 GUI's in-app update checks —
neither path consumes the GitHub Auth API, so no auth, no rate limit.

Source code for these artifacts lives in private repos
(`RehabtekLLC/AR01_FW`, `RehabtekLLC/AR01-GUI`). This repo only carries
release outputs.

> **The full release architecture — publish flows, tag conventions,
> manifest schemas, troubleshooting, retirement plans — is documented
> in
> [`AR01-GUI/docs/release-architecture.md`](https://github.com/RehabtekLLC/AR01-GUI/blob/main/docs/release-architecture.md).
> The doc lives next to the workflows that produce these artifacts so
> they stay in sync. Read it first when debugging a release.**

## Layout

```
AR01_Updates/
├── fw/
│   ├── manifest.json      Current FW version + binary URLs + SHA-256
│   ├── esp32-nexus.bin    Latest ESP32 firmware (always overwritten)
│   └── stm32-app.bin      Latest STM32 firmware (always overwritten)
└── gui/
    └── manifest.json      All GUI versions: latest + history array
```

Firmware binaries are committed into `fw/` AND attached to tagged
releases. GUI installers are **not** committed into `gui/` — they live
exclusively as release assets under `gui-v*` tags on this repo, with
`gui/manifest.json` as the canonical index.

## Quick reference

**Manifest URLs** (raw, anonymous, no API auth):

- Firmware:
  `https://raw.githubusercontent.com/RehabtekLLC/AR01_Updates/main/fw/manifest.json`
- GUI:
  `https://raw.githubusercontent.com/RehabtekLLC/AR01_Updates/main/gui/manifest.json`

**Tag conventions**:

| Source                               | Source tag    | Release tag on this repo |
|--------------------------------------|---------------|--------------------------|
| Firmware (`AR01_FW`)                 | `v1.2.3`      | `v1.2.3`                 |
| GUI (`AR01-GUI`)                     | `gui-v1.2.3`  | `gui-v1.2.3`             |

There is also a transitional **legacy GUI mirror** on
`RehabtekLLC/AR01-Nexus-GUI-releases` that publishes the same installer
under the bare `v1.2.3` tag (no `gui-` prefix) so older in-field
installs whose update check can't parse the new prefix keep receiving
updates. That mirror retires once the field has rolled forward — see
the architecture doc for the cutover plan.

## When something breaks

Don't debug from scratch — the symptom-first troubleshooting section
in
[release-architecture.md#troubleshooting](https://github.com/RehabtekLLC/AR01-GUI/blob/main/docs/release-architecture.md#troubleshooting)
covers the failure modes we've actually hit, including:

- in-field installs not seeing a new release
- update prompts that fire but fail to download/install
- workflows that go green but don't publish
- `gui/manifest.json` not updating
- firmware OTA failing mid-flash
- `release.yml` version-mismatch errors
