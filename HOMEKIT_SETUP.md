# HomeKit Bridge Setup

## Step 1 — Add to configuration.yaml

```yaml
homekit: !include homekit.yaml
```

## Step 2 — Clear old bridge state (important after previous attempts)

In HA, go to **Developer Tools → Template** and check for stale `.storage/homekit.*`
files, OR use the File Editor / SSH addon to delete:

```
/config/.storage/homekit.XXXX       (pairing data)
/config/homekit.XXXX.state          (entity state cache)
```

Delete any files matching those patterns before restarting.

## Step 3 — Restart HA and pair

1. Restart Home Assistant
2. Go to **Settings → Integrations → HomeKit Bridge**
3. A QR code / PIN will appear — scan it with the iPhone Camera app
4. The bridge will appear as **"HA Bridge"** in Apple Home with 16 accessories:
   - 2 garage doors
   - 10 water sensors
   - 4 outlets

## Step 4 — Ecobee native HomeKit (do this separately)

Your Ecobee thermostat has HomeKit built in. Pair it directly:

1. Open the **Ecobee app** on your phone
2. Go to **Main Menu → Settings → HomeKit**
3. Follow the pairing flow — it shows a HomeKit code
4. In Apple Home, add accessory → enter the code

The Ecobee HA integration keeps running in parallel. You'll have:
- Thermostat control + temperature + humidity in Apple Home (via native Ecobee HomeKit)
- CO₂, VOC, AQI, occupancy sensors available in HA automations (via HA Ecobee integration)

## Step 5 — Ring (optional)

Ring Alarm base stations support HomeKit natively (contact/motion sensors, siren).
Ring cameras do NOT officially support HomeKit — use the Ring app for those.
The Ring HA integration stays in place for `last_activity` automation triggers.

## What's intentionally excluded from the HA bridge

| Category | Reason |
|---|---|
| Ecobee climate/sensors | Paired natively to Apple Home |
| Ring camera / chimes | Ring app / native HomeKit is better |
| ZHA opening/tamper sensors | Not useful in Apple Home; noise |
| ZHA battery sensors | Available in HA only |
| ZHA power monitoring | Too granular for Apple Home |
| ZHA siren switches | Avoid accidental triggers |
| ZHA diagnostic sensors | LQI/RSSI — HA only |
| Firmware updates | HA only |
| Buttons / numbers | HA only |
