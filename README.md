# OmniCAN Gateway OTA

Public repository for OmniCAN Gateway ESP32 firmware releases.

## Overview

This repository contains OTA (Over-The-Air) update files for the OmniCAN Gateway ESP32-S3 device. The OmniCAN mobile app checks this public repository for firmware updates, allowing seamless OTA updates without exposing the main private repository.

## Release Process

### Creating a New Release

1. **Build the firmware** using PlatformIO or Arduino IDE
2. **Generate the firmware binary** (`.bin` file)
3. **Create a GitHub Release**:
   - Go to [Releases](https://github.com/BowerMotorsport/OmniCAN_Gateway_OTA/releases)
   - Click "Draft a new release"
   - Set the tag version (e.g., `v1.0.0`)
   - Upload the following assets:
     - `<version>.bin` - The ESP32 firmware binary
     - `manifest.json` - Update metadata (see format below)

### manifest.json Format

Create a `manifest.json` file with the following structure:

```json
{
  "latest": {
    "version": "1.0.0",
    "download_url": "https://github.com/BowerMotorsport/OmniCAN_Gateway_OTA/releases/download/v1.0.0/firmware.bin",
    "sha256": "abc123...",
    "size": 1234567,
    "release_notes": "Bug fixes and improvements",
    "release_date": "2025-01-09T12:00:00Z"
  }
}
```

**Fields:**

| Field | Description |
|-------|-------------|
| `version` | Version string (e.g., "1.0.0") |
| `download_url` | Direct link to the `.bin` file |
| `sha256` | SHA256 checksum of the binary (lowercase) |
| `size` | Binary size in bytes |
| `release_notes` | (Optional) Release notes |
| `release_date` | (Optional) ISO 8601 date string |

### Generating SHA256

**Windows (PowerShell):**
```powershell
Get-FileHash -Algorithm SHA256 firmware.bin | Format-List
```

**macOS/Linux:**
```bash
sha256sum firmware.bin
```

## Binary Naming

Use the following naming convention for binary files:
- `firmware_v1.0.0.bin` or simply `v1.0.0.bin`

## App Integration

The OmniCAN mobile app automatically:
1. Fetches `manifest.json` from the latest release
2. Compares versions with the connected ESP32
3. Downloads firmware if an update is available
4. Transfers firmware via BLE OTA protocol

## Technical Details

- **Protocol**: BLE OTA (custom implementation)
- **BLE Service UUID**: `00002234-0000-1000-8000-00805f9b34fb`
- **Max Chunk Size**: 240 bytes per BLE write
- **Verification**: CRC32 during transfer, SHA256 checksum verification

## Support

For issues with OTA updates, check:
1. Device is within BLE range
2. Device battery is sufficient (>20%)
3. No interference on BLE advertising channel

## License

Proprietary - Bower Motorsport
