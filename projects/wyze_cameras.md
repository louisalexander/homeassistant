# Wyze Camera Fleet — Flash & HA Integration

## Hardware Inventory

| # | Model | Color | Sensor | HA Entity | Status |
|---|---|---|---|---|---|
| Cam 1 | Wyze Cam v2 | White | jxf23 | `camera.wyze_cam_v2_1` | **Trashed** — discarded 2026-05-25 |
| Cam 2 | Wyze Cam v2 | White | jxf23 | `camera.wyze_2` | **Trashed** — discarded 2026-05-25 |
| Cam 3 | Wyze Cam v2 | White | jxf23 | `camera.wyze_cam_v2_3` | **Trashed** — discarded 2026-05-25 |
| Cam 4 (black) | Wyze Cam v2 | Black (WYZEC2BK) | Unknown | — | **Trashed** — was bricked (mtd0 corrupt); discarded 2026-05-25 |
| Wyze v3 | Wyze Cam v3 | — | gc2053 | `camera.wyze_cam_v3_1` | Flashed (wltechblog installer), in HA — placement TBD |

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

### v2 Flash Steps (Confirmed Working Process)

Two-stage flash required for Wyze v2:

**Stage 1 — Flash Thingino bootloader via wz_mini initramfs (Raspberry Pi)**
1. Write `wz_mini_u_boot_initramfs.img` to SD card using Raspberry Pi Imager
2. Insert into camera, hold setup button, power on — hold button until LED changes
3. Wait ~60s for bootloader to flash. SD card gets `wz_mini_u_boot_initramfs.log` when done
4. Camera reboots automatically

**Stage 2 — Flash Thingino firmware**
1. Download correct firmware from GitHub release `firmware-2026-05-18`:
   - jxf23 (most v2 white): `thingino-wyze_cam2_t20x_jxf23_rtl8189ftv.bin` — **8,388,608 bytes**
   - jxf22 (some v2): `thingino-wyze_cam2_t20x_jxf22_rtl8189ftv.bin` — 4,651,205 bytes
2. Rename correct file to `autoupdate-full.bin` on the SD card
3. Delete `autoupdate-full.done` if present
4. Insert SD card, power on (no button hold) — camera flashes firmware, LEDs cycle, then go white/blue
5. Connect to `thingino-XXXXXX` AP → `192.168.1.1` → enter home WiFi credentials
6. Camera joins home network

**Key lessons learned:**
- File must be named `autoupdate-full.bin` (not `.yay` or anything else)
- If `autoupdate-full.done` exists, bootloader skips flash entirely — delete it to reflash
- jxf23 firmware must be exactly 8,388,608 bytes — a 2.1MB file is a corrupt/incomplete build
- jxf22 wrong sensor = camera boots but gets stuck (yellow LED, no WiFi AP)
- No LED after 30s = mtd0 corrupt, needs UART/USB bootrom recovery
- RTSP credentials are separate from web UI credentials — find via Thingino web UI direct endpoints page
- Generic Camera in HA must be added via UI (not YAML) in recent HA versions
- Snapshot endpoint is `/x/ch0.jpg` but requires session cookie/API key — use RTSP only for HA

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
| Wyze v3 #1 | v3 | TBD — back door/mudroom or rear yard | IP65-rated; can go outdoor as Ring supplement, or cover mudroom entry |

> All v2 units trashed 2026-05-25 — image quality too poor. v3 is the only remaining Wyze unit.

---

## HA Dashboard Integration

After adding cameras, add to dashboard:
- **Picture Glance card** per camera — shows live thumbnail
- Group all Wyze cameras under a "Cameras" tab or section on the Home Command Dashboard

---

## Open Questions

- [ ] Decide placement for v3: back door/mudroom interior, or outdoor rear yard
- [ ] Confirm outlet/power at chosen v3 location
- [ ] Decide on motion detection approach: Thingino local motion, or pipe RTSP into HA Frigate/MotionEye (more complex, defer for now)

---

## Future: Frigate NVR (Optional, Deferred)

If local AI object detection becomes useful (person detection, vehicle detection), [Frigate](https://frigate.video) is the standard HA add-on. It consumes the RTSP streams from all cameras (Ring + Wyze) and runs local inference. Requires a Coral Edge TPU or beefy CPU. Not needed initially — standard camera cards + Ring motion sensors are sufficient for now.

*Last updated: 2026-05-25*
*Status: All v2 units trashed (image quality); 1× v3 in HA, placement TBD*
