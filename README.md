# EnviroNode firmware

Signed firmware images for EnviroNode sensors and the manifest the EnviroNode
app reads to offer updates over Bluetooth. The source is in a separate
(private) repository; images are published here with
`tools/release_firmware.py` from that repository.

## Channels

One branch per channel:

| Branch | Who gets it |
|---|---|
| `main` | every app (stable) |
| `staging` | test builds of the app (`--dart-define=FW_CHANNEL=staging`): release candidates are checked on real nodes here before they go to `main` |

## firmware.json

A list, newest version first:

```json
[
  {
    "version": "0.3.1",
    "board": "envnode_v1",
    "file": "envnode_v1_0.3.1.bin",
    "date": "2026-10-26",
    "size": 260240,
    "sha256": "…",
    "min_node": "0.3.0",
    "description": "…"
  }
]
```

| Field | Meaning |
|---|---|
| `version` | image version (MCUboot header) |
| `board` | `envnode_v1` (EnviroNode board) or `nrf54l15dk` (development kit); the app offers only images for the node's board |
| `file` | image in this branch, `<board>_<version>.bin` |
| `size`, `sha256` | checked by the app after the download |
| `min_node` | oldest firmware a node may run to take this image over Bluetooth; older nodes need a wired update (e.g. 0.3 changed the flash layout) |
| `description` | release notes shown in the app |

Images are MCUboot images signed with the EnviroNode key (Ed25519). A node
installs an update in test mode and keeps it only after three good
measurements; otherwise MCUboot rolls it back.
