# BitDisplay OTA

Public firmware distribution repository for supported BitDisplay devices by Houses2521.

BitDisplay products are designed, built, and maintained in Thailand. This repository provides the OTA firmware files and version metadata used by compatible devices in the field.

## Public Scope

This repository contains public release artifacts only:

- Model-specific `firmware.bin` files
- `version.txt` release metadata
- This public documentation

It does not contain source code, secrets, API keys, customer data, manufacturing records, or device provisioning information.

## How OTA Updates Work

A compatible BitDisplay device checks its assigned OTA channel when it has a stable network connection.

1. The device reads `version.txt`.
2. It compares the published version with its installed firmware.
3. When the release is applicable, it downloads `firmware.bin` from the same channel.
4. The device applies the update and restarts.

If an update check or download cannot be completed, the device continues running its installed firmware and retries later.

## Public OTA Channels

Each active product channel has its own folder containing `version.txt`.
`firmware.bin` is present only when an artifact has been deliberately
published for that channel:

```text
version.txt
firmware.bin  # optional until a release artifact is published
```

Current public state, verified 2026-08-26:

| BitDisplay model | Display / firmware line | OTA folder | Metadata | Binary |
| --- | --- | --- | --- | --- |
| O1 | BTC/USDT display | `O1/` | `2\|NONE` | published FW2 |
| U1 | BTC/EUR display | `U1/` | `3\|NONE` | published |
| G1 | XAU/USDT display | `G1_V2/` | `7\|NONE` | published |
| TG | Thai gold display | `TG/` | `15\|NONE` | published |
| TG2 | Thai gold buy/sell display | `TG2/` | `2\|NONE` | published |
| TG3 | Compact Thai gold buy/sell display | `TG3/` | `1\|NONE` | **not published** |
| TG4 | Thai gold buy/sell display | `TG4/` | `5\|NONE` | published |
| T1 | Time display | `T1/` | `3\|NONE` | published |
| O2 | BTC/THB display | `BTCTHB/` | `6\|NONE` | published |
| G2 | XAU/THB display | `XAUTTHB/` | `3\|NONE` | published |
| CT | USDC/THB display | `USDCTHB/` | `6\|NONE` | published |

All listed channels are closed at `NONE`; no OTA rollout was active at the
verification time. TG3 is currently metadata-only: a
device can read its version metadata, but no TG3 application image is currently
available at the documented binary URL.

Some additional folders may be retained for compatibility with earlier deployed devices or for controlled testing. They are not public product-release channels.

## `version.txt` Format

The preferred format is one line:

```text
<version>|<scope>
```

Examples:

```text
7|NONE
7|ALL
```

| Scope | Meaning |
| --- | --- |
| `NONE` | No rollout is currently active, even if a binary remains published. |
| `ALL` | The release is available to compatible devices in that channel. |

Earlier firmware versions may also accept a plain integer version. Publication and any controlled rollout scope are managed by Houses2521.

The firmware source version and the published OTA version are separate release
states. For example, O1 source may advance while its OTA channel intentionally
retains an earlier accepted binary. Do not infer one from the other.

## Public URL Pattern

For any listed OTA folder:

```text
https://raw.githubusercontent.com/OFFOCUS/BitDisplay-OTA/main/<channel>/version.txt
https://raw.githubusercontent.com/OFFOCUS/BitDisplay-OTA/main/<channel>/firmware.bin
```

Example for O1:

```text
https://raw.githubusercontent.com/OFFOCUS/BitDisplay-OTA/main/O1/version.txt
https://raw.githubusercontent.com/OFFOCUS/BitDisplay-OTA/main/O1/firmware.bin
```

## Release Safety

Firmware is channel-specific. A published `firmware.bin` must match the intended BitDisplay model, board configuration, display wiring, partition layout, and firmware line.

For this reason:

- Do not manually flash a binary intended for another model or channel.
- Do not rename or move channel folders used by deployed devices.
- Publish a tested binary before increasing its version metadata.
- Use a controlled rollout before making a new production release broadly available.

## Maintainer

Houses2521  
BitDisplay product line  
Made in Thailand  
ESP32 IoT display firmware and OTA distribution
