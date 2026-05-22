# Wyze Camera Fleet — Flash & HA Integration

## Hardware Inventory

| Model | Qty | Outdoor-rated | Night Vision | Status |
|---|---|---|---|---|
| Wyze Cam v2 | 2+ | No (indoor only) | IR only | Not flashed |
| Wyze Cam v3 | 1+ | Yes (IP65) | Color Starlight + IR | Not flashed |

> Count TBD — confirm exact unit count before proceeding.

---

## Goal

Replace cloud-dependent Wyze app with local RTSP streams that feed directly into HA. No Wyze account required after flashing. Cameras appear as generic camera entities in HA.

---

## Flash Approach

### Recommended Firmware: Thingino

[Thingino](https://thingino.com) is an open-source firmware project (based on OpenIPC) with active support for both Wyze v2 and v3. It provides:
- Local RTSP stream (no cloud, no subscription)
- Web UI for camera config
- ONVIF support
- Motion detection (local, no cloud)
- MQTT integration option

**Why Thingino over Wyze's official RTSP firmware:**
- Wyze's RTSP firmware is old, essentially unmaintained
- Thingino is actively developed with a community
- Works on both v2 and v3 with the same tooling

### v2 Flash Steps

1. Download Thingino image for Wyze Cam v2 from [thingino.com/supported-devices](https://thingino.com/supported-devices)
2. Write image to a micro-SD card (8GB+, FAT32)
3. Insert SD card into powered-down camera
4. Power on — camera boots from SD and flashes itself
5. Camera reboots, joins Wi-Fi (configure via Thingino web UI on the camera's IP)
6. Enable RTSP in Thingino settings
7. RTSP URL: `rtsp://[camera-ip]:554/ch0` (confirm in Thingino docs)

### v3 Flash Steps

Same SD card method. The v3 uses a different image file — confirm the exact Thingino image name for Wyze v3 from the supported devices page. The v3 SD card slot is under the magnetic base.

**v3 outdoor advantage:** After flashing, the v3 retains its IP65 weather rating. Can be placed outdoors as a supplementary camera alongside or instead of Ring outdoor units.

---

## HA Integration

Once RTSP is live, add each camera as a Generic Camera in HA:

```yaml
# In configuration.yaml (or via UI: Settings → Integrations → Generic Camera)
camera:
  - platform: generic
    name: "Wyze Cam v2 — Office"
    stream_source: "rtsp://192.168.x.x:554/ch0"
    verify_ssl: false
```

Or use the **Generic Camera** integration via HA UI (Settings → Integrations → Add Integration → Generic Camera) — no YAML needed.

**Stream performance note:** RTSP streams are pulled over local LAN — no cloud latency. HA can display them in a dashboard Picture Glance or Camera card.

---

## Placement Plan

| Camera | Model | Proposed Location | Notes |
|---|---|---|---|
| Wyze v3 #1 | v3 | Outdoor — rear yard / patio area | IP65-rated; supplements Ring outdoor coverage |
| Wyze v2 #1 | v2 | Indoor — garage interior | Monitors inside garage; plug-in near garage outlet |
| Wyze v2 #2 | v2 | Indoor — TBD (laundry? mudroom?) | Interior coverage where a plug-in cam is useful |

> Adjust once exact unit count is confirmed. v3 cameras (outdoor-capable) are higher value outdoors than indoors given the Ring cameras already cover indoor spots.

---

## HA Dashboard Integration

After adding cameras, add to dashboard:
- **Picture Glance card** per camera — shows live thumbnail
- Group all Wyze cameras under a "Cameras" tab or section on the Home Command Dashboard

---

## Open Questions

- [ ] Confirm exact unit count: how many v2, how many v3?
- [ ] Check if any cameras still have Wyze stock firmware and whether they're currently in use
- [ ] Confirm Wi-Fi coverage at each proposed location (Wyze cameras are 2.4 GHz only)
- [ ] Decide on motion detection approach: Thingino local motion, or pipe RTSP into HA Frigate/MotionEye (more complex, defer for now)

---

## Future: Frigate NVR (Optional, Deferred)

If local AI object detection becomes useful (person detection, vehicle detection), [Frigate](https://frigate.video) is the standard HA add-on. It consumes the RTSP streams from all cameras (Ring + Wyze) and runs local inference. Requires a Coral Edge TPU or beefy CPU. Not needed initially — standard camera cards + Ring motion sensors are sufficient for now.

*Last updated: 2026-05-21*
*Status: Planning — cameras not flashed; count TBD*
