# BitDisplay-OTA  
Firmware Distribution Server for BitDisplay (O1 / G1 / U1)

This repository serves as the official OTA (Over-The-Air) firmware distribution system for all BitDisplay models developed under Houses2521.

It is designed for:

- Safe firmware rollout
- Targeted device updates
- Scalable multi-device deployment
- Long-runtime stability (24/7 operation)

---

# 📦 Supported Models

| Model | Product | API Source | OTA Folder |
|-------|--------|------------|------------|
| O1 | BTC.$ (USD) | Binance | `/O1/` |
| U1 | BTC.€ (EUR) | Binance | `/U1/` |
| G1 | XAU.$ (Gold) | CoinGecko / Binance | `/G1/` |
| G1_V2 | XAU.$ (Binance Futures) | Binance Futures | `/G1_V2/` |

Each model folder contains:

- `firmware.bin`
- `version.txt`

---

# 📁 Repository Structure

```
BitDisplay-OTA/
│
├── O1/
│   ├── firmware.bin
│   └── version.txt
│
├── U1/
│   ├── firmware.bin
│   └── version.txt
│
├── G1/
│   ├── firmware.bin
│   └── version.txt
│
└── G1_V2/
    ├── firmware.bin
    └── version.txt
```

---

# 🔄 OTA Update Flow (Device Side Logic)

Each BitDisplay device performs:

1. Request `version.txt`
2. Compare remote version with local `FW_VERSION`
3. Validate DEVICE_ID against target list
4. Download `firmware.bin`
5. Flash firmware
6. Restart device

OTA will only execute if:

- WiFi is connected
- Device is in stable RUN_CONNECTED state
- Remote version is higher than local version
- DEVICE_ID is allowed in target list

---

# 📄 version.txt Format

### Update ALL devices
```
5
```
or
```
5|ALL
```

### Update specific devices only
```
5|88,90,102
```

### Block update
```
5|NONE
```

Format rule:

```
<version>|<target list>
```

Target list options:
- `ALL`
- `NONE`
- Comma-separated DEVICE_ID values

---

# 🛠 How To Release New Firmware

### Step 1 — Compile firmware
Export `.bin` from Arduino IDE or VSCode.

### Step 2 — Upload firmware
Replace:

```
/ModelName/firmware.bin
```

Example:
```
/O1/firmware.bin
```

### Step 3 — Update version.txt
Increase version number:

Example:
```
6|ALL
```

### Step 4 — Commit to main branch

Devices will update automatically within their OTA check interval.

---

# 🧠 Device Identity System

Each device contains:

```
uint16_t deviceId
```

- Stored in NVS (Preferences)
- Provisioned once via USB
- OTA updates DO NOT overwrite deviceId

Provision example in firmware:

```
#define PROVISION_DEVICE_ID  88
```

After first flash, identity persists permanently.

---

# 🌐 OTA Source URL Pattern

Example (O1):

```
https://raw.githubusercontent.com/OFFOCUS/BitDisplay-OTA/main/O1/version.txt
https://raw.githubusercontent.com/OFFOCUS/BitDisplay-OTA/main/O1/firmware.bin
```

All models follow the same pattern.

---

# 🔒 Production Rollout Strategy

Recommended staged deployment:

### Phase 1 — Test devices
```
7|12,15,18
```

### Phase 2 — Observe stability

### Phase 3 — Release to all
```
7|ALL
```

This prevents mass device failure in case of unexpected bugs.

---

# ⚙ OTA Safety Features (Implemented in Firmware)

- DEVICE_ID stored in NVS
- Deterministic stagger (avoid server spike)
- Boot-time OTA check
- Periodic OTA check
- WiFi stability guard
- OTA failure backoff
- Watchdog-safe download loop
- Targeted device control

---

# ⚡ System Design Philosophy

BitDisplay OTA infrastructure is built for:

- 24/7 always-on displays
- Low-power IoT devices
- Internet instability tolerance
- Mass commercial deployment
- Safe remote firmware management

---

# 📌 Important Notes

- Devices must remain online to receive OTA
- Always increment `FW_VERSION` before publishing new firmware
- Ensure firmware.bin matches correct model folder
- Never modify deviceId via OTA
- Use staged rollout for production stability

---

# 🏷 Maintainer

Houses2521  
Embedded Systems & IoT Display Development  
ESP32 + HUB75 + OTA Infrastructure  

---

# 🎯 Commercial Impact

This OTA system enables:

- Zero-touch firmware updates
- Remote bug fixes
- Feature expansion without hardware recall
- Scalable multi-country deployment
- Controlled staged firmware release

---

BitDisplay OTA is not just a firmware host —  
it is a controlled deployment infrastructure for production IoT devices.
