# BitDisplay OTA

Official public firmware distribution endpoint for BitDisplay devices by Houses2521.

BitDisplay is designed, built, and maintained in Thailand as part of the Houses2521 product line.

This repository is used by supported BitDisplay products to check for firmware availability and download OTA firmware files. It is intentionally small and public-facing: it contains release artifacts and simple version metadata only.

## Public Scope

This repository may contain:

- OTA firmware binaries for supported BitDisplay channels
- `version.txt` metadata for each channel
- Public release notes or operational notes when needed

This repository should not contain:

- Source code
- Secrets, API keys, tokens, or private configuration
- Customer information
- Internal manufacturing or provisioning records

## OTA Channels

Each OTA channel has its own folder. A folder normally contains:

```text
firmware.bin
version.txt
```

Current public channel folders:

| Channel | Product / Firmware line | Folder |
| --- | --- | --- |
| O1 | BitDisplay O1, BTC/USD display | `O1/` |
| U1 | BitDisplay U1, BTC/EUR display | `U1/` |
| G1 | BitDisplay G1 legacy gold display | `G1/` |
| G1_V2 | BitDisplay G1 current gold display | `G1_V2/` |
| TG | BitDisplay Thai gold display | `TG/` |
| BTCTHB | BitDisplay O2, BTC/THB display | `BTCTHB/` |
| XAUTTHB | BitDisplay G2, gold/THB display | `XAUTTHB/` |
| USDCTHB | BitDisplay CT, USD/THB display | `USDCTHB/` |

## Device Update Flow

A supported BitDisplay device periodically checks its assigned channel:

1. Read `version.txt` from the channel folder.
2. Compare the remote firmware version with the version currently running on the device.
3. Confirm that the rollout scope allows the device to update.
4. Download `firmware.bin` from the same channel.
5. Apply the update and restart.

Devices are expected to update only when network and runtime conditions are safe enough for OTA. If a check fails, the device continues running the current firmware and retries later.

## `version.txt` Format

`version.txt` uses one line:

```text
<version>|<rollout_scope>
```

Examples:

```text
5|NONE
5|ALL
```

Rollout scope values:

| Value | Meaning |
| --- | --- |
| `NONE` | No public rollout is active for this version. |
| `ALL` | Release is available to all eligible devices on that channel. |
| Internal rollout value | Used by Houses2521 for controlled staged rollout. |

A plain integer version may also be supported by older firmware, but the preferred public format is `version|scope`.

## Public URL Pattern

Example for the O1 channel:

```text
https://raw.githubusercontent.com/OFFOCUS/BitDisplay-OTA/main/O1/version.txt
https://raw.githubusercontent.com/OFFOCUS/BitDisplay-OTA/main/O1/firmware.bin
```

Other channels follow the same pattern using their folder name.

## Release Safety Notes

For maintainers, each firmware release should follow a conservative process:

1. Build the firmware for the exact BitDisplay model and hardware configuration.
2. Test the binary on matching hardware before publishing.
3. Upload the matching `firmware.bin` to the correct channel folder.
4. Increase the firmware version in `version.txt` only after the binary is ready.
5. Use `NONE` or a controlled rollout first when validating a new release.
6. Move to `ALL` only after the release is stable.

Never publish a binary to the wrong channel. A firmware file must match the board, display wiring, partition layout, libraries, and product behavior expected by that channel.

## Reliability Principles

BitDisplay OTA is designed around practical field reliability:

- Small, predictable public files
- Separate channel per product line
- Staged rollout support
- Graceful retry behavior on device side
- No secrets or private operational data in the public repository
- Made in Thailand hardware and firmware ownership

## Maintainer

Houses2521  
Made in Thailand  
BitDisplay product line  
ESP32 IoT display firmware and OTA distribution
