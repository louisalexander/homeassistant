# Camera System

## Coverage Strategy

| Layer | Technology | Purpose |
|---|---|---|
| **Perimeter** | Ring (cloud) | Exterior entrances, driveway, doorbell — cloud-recorded under Ring Protect Pro |
| **Interior** | Wyze + Thingino (local) | Interior spaces — RTSP stream, no cloud, no subscription |

---

## Active Cameras

| Camera | Location | Entity | Stream |
|---|---|---|---|
| Ring Indoor Cam | Garage interior | `camera.garage_stickup_cam_1_live_view` | Cloud on-demand |
| Ring Indoor Cam | Personnel door (house side) | `camera.personnel_door_live_view` | Cloud on-demand |
| Wyze Cam v3 (Thingino) | Office — 3rd floor | `camera.wyze_cam_v3_1` | Local RTSP — always live |

---

## Pending Cameras

| Camera | Location | Status |
|---|---|---|
| Ring Outdoor Cam + Solar Panel × 2 | Driveway + rear — exact spots TBD | Sun exposure walk needed before mounting |
| Ring Video Doorbell Powered 2K | Front door | Hardware in hand; transformer in hand |

---

## Ring Chimes

4× Ring Chime V3 placed and active as HA `siren` entities:

| Chime | Location | Entity |
|---|---|---|
| Kitchen | 1st floor kitchen | `siren.kitchen_chime` |
| Foyer | 1st floor foyer | `siren.foyer_chime` |
| Hallway | 2nd floor hallway | `siren.hallway_siren` |
| Office | 3rd floor office | `siren.office_siren` |

---

## Dashboard — Security Tab

Three camera tiles, all full-width:

| Tile | Card type | Camera view | Notes |
|---|---|---|---|
| Personnel Door Cam | `picture-glance` | `auto` (snapshot) | Ring cloud — refreshes on motion + every 2 min |
| Garage Cam | `picture-glance` | `auto` (snapshot) | Ring cloud — refreshes on motion + every 2 min |
| Office Cam | `picture-entity` | `live` | Local RTSP — always streaming |

### Ring Snapshot Refresh

Ring tiles use `camera_view: auto` (not `live`) because Ring cameras are cloud on-demand — attempting `live` results in a stalled loading state. Snapshots update:
1. **On motion** — Ring pushes a new snapshot when it detects motion
2. **Every 2 minutes** — Section 13 automation calls `homeassistant.update_entity`

### Ring Motion Chime

Section 14 automations ring all 4 chimes on motion from either Ring camera, with a 10-minute cooldown per camera. Triggered by `sensor.[cam]_last_activity` timestamp change (Ring doesn't expose a motion `binary_sensor` for these models).

!!! tip "Disable Ring's built-in notifications"
    Ring's app sends its own push notifications and rings chimes independently. To avoid double-alerting, disable Motion Alerts per-device in the Ring app, and unlink cameras from chimes in Ring Chime settings.

---

## Wyze Cam v3 — Office

- **Firmware:** Thingino (open-source OpenIPC-based firmware)
- **Hostname:** `ing-wyze-cam3-3a32`
- **Stream:** Local RTSP — `rtsp://[camera-ip]:554/ch0`
- **HA integration:** Generic Camera integration

### Motion Detection (Optional — Not Currently Active)

Infrastructure is in place but not enabled:

- **HA entity:** `binary_sensor.office_cam_motion` (defined in `configuration.yaml`, 30s auto-off)
- **MQTT topic:** `thingino/ing-wyze-cam3-3a32/motion` (assumed — verify before enabling)

To activate:

1. Open Thingino web UI at camera IP
2. MQTT settings → set broker to HA's local IP, port 1883
3. Enable motion detection + MQTT publish
4. Verify topic: Developer Tools → MQTT → Listen to `#` → walk in front of camera
5. Update `state_topic` in `configuration.yaml` if topic differs; restart HA

---

## Frigate NVR (Future / Deferred)

AI object detection (person vs. animal vs. vehicle) is not needed currently. If it becomes useful:

- **Software:** Frigate NVR — HA add-on, consumes RTSP streams
- **Hardware:** Coral USB Accelerator (G950-01456-01, ~$60) — USB 3.0, plugs into Mini S13
- **Source:** Check Mouser/DigiKey for MSRP stock before Amazon

Not worth pursuing until there's a concrete use case (e.g. "notify only when a *person* is in the garage, not a car").

---

## Ring Subscription

**Ring Protect Pro** — 1-year subscription purchased 2026-05-21 (renews ~2027-05-21).
Covers all Ring devices at this location — video history, cloud recording, professional monitoring.
