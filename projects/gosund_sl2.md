# Gosund SL2 LED Strip — Flash & HA Integration

## Status
On hold — device can't be located. Document is a placeholder for when it's found.

## Hardware
**Gosund SL2** — LED light strip with inline controller box.

## Flash Path

The SL2 is likely ESP8266-based (common in Gosund strip controllers of this era), which makes Tasmota flashing straightforward. Verify before flashing:

1. Look up "Gosund SL2" on [templates.blakadder.com](https://templates.blakadder.com)
2. If ESP8266/ESP8285: flash Tasmota via USB or Tuya Convert OTA
3. If CB3S / BK7231N: use Tuya Cloudcutter (more involved, but still doable)

**If ESP-based (expected):**
- Flash Tasmota using [Tasmota Web Installer](https://tasmota.github.io/install/) (Chrome, USB, no soldering needed if USB access is available on the controller)
- Or use Tuya Convert if still unregistered in Tuya app (OTA, no disassembly)
- Apply Tasmota LED strip template from blakadder.com for SL2

**HA integration after flashing:**
- Tasmota auto-discovered via MQTT (Mosquitto broker already installed)
- Exposes: `light.[name]` with RGB + brightness + color temp if applicable
- Adaptive Lighting HACS integration can take over once entity is live

## Placement TBD
No placement decision made — decide when device is found.

*Last updated: 2026-05-21*
*Status: On hold — device not located*
