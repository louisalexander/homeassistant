# Camera Plan — Unified Coverage Strategy

## Camera Inventory

| Camera | Qty | Type | Night Vision | Power | Cloud | Status |
|---|---|---|---|---|---|---|
| Ring Video Doorbell Powered 2K | 1 | Outdoor | Color | Hardwired | Ring | Not installed |
| Ring Outdoor Cam + Solar Panel | 2 | Outdoor | Color | Solar | Ring | Not mounted |
| Ring Indoor Cam (plug-in) | 2 | Indoor | IR (B&W) | Plug-in | Ring | Cam 1: Garage — active; Cam 2: Personnel door — active |
| Wyze Cam v2 | 0 | — | — | — | — | **Trashed** — all units discarded 2026-05-25 (poor image quality) |
| Wyze Cam v3 | 1 | Indoor/Outdoor (IP65) | Color Starlight | Plug-in | Local (Thingino) | Flashed, in HA — placement TBD |

---

## Coverage Philosophy

**Ring cameras = perimeter + entrance layer**
- Cloud-recorded under Ring Protect Pro (already purchased)
- Integrated with Ring app for real-time alerts and clips
- Solar outdoor cams go where wiring is hard; doorbell is hardwired
- HA integration via Ring integration — motion entities, camera streams

**Wyze cameras (post-Thingino) = interior + supplemental layer**
- Fully local after Thingino firmware flash — no cloud, no subscription
- RTSP stream → HA Generic Camera integration
- Private: footage stays on local network
- Best for interior spaces where Ring's cloud model is overkill

---

## Proposed Camera Assignments

### Ring — Perimeter Layer

| Camera | Location | Rationale |
|---|---|---|
| Ring Doorbell Powered 2K | Front door | Primary entrance; hardwired; covers front porch and approach |
| Ring Outdoor Solar Cam 1 | Driveway / front yard — secondary angle | Different FOV from doorbell; captures vehicles; needs south/east/west downspout with 4+ hrs sun |
| Ring Outdoor Solar Cam 2 | Rear of house — back door / yard | Covers rear entry point; confirm sun exposure before mounting |
| Ring Indoor Cam (cam 1) | Garage interior | **Keep as-is** — covers most-used entry point; ESPHome controls doors but no visual without this |
| Ring Indoor Cam (cam 2) | Personnel door — house side | Covers the chokepoint between garage and house interior; captures faces at entry; complements garage cam which covers the space. Mount on house side of door aimed at anyone walking through. **Confirm outlet exists near door before mounting.** |

### Wyze — Interior Layer

| Camera | Location | Rationale |
|---|---|---|
| Wyze v3 | TBD — back door/mudroom or rear yard | Color night vision + IP65; good at a drafty mudroom entry or as an outdoor supplement behind the house |

> All v2 units trashed 2026-05-25. One v3 remains — placement decision pending.

---

## Sun Exposure Check (before mounting Ring outdoor cams)

Both Ring solar cams need **4+ hours of direct sun** daily to stay charged. Before committing to downspout locations:

- [ ] Walk the exterior and identify which downspouts face south, east, or west
- [ ] Check for roofline overhang or tree shade that would cut solar hours
- [ ] Confirm camera elevation gives useful view angle (8–10 ft typical)
- [ ] Confirm Wi-Fi signal at each proposed location (Ring Chime Pro can extend if needed — but deprecated; use router placement instead)

If rear of house has poor sun exposure, Ring Outdoor Cam 2 may need a wired power source instead of solar — check if an outdoor outlet is accessible.

---

## Coverage Gaps

After full deployment, these zones remain uncovered:

| Zone | Gap | Option |
|---|---|---|
| Side yard | Neither Ring outdoor cam covers it unless aimed there | Sacrifice one Ring cam angle or add a Wyze v3 outdoors if outlet available |
| 2nd/3rd floor interior | No interior cams planned above 1st floor | Add Wyze v2 if count allows; low priority |
| Front room / living area | Interior main floor uncovered when away | Add Wyze v2 if count allows |

---

## HA Integration

### Ring cameras
Already active via Ring integration. New devices auto-appear once registered.
```
binary_sensor.[camera]_motion  → triggers automations
camera.[camera]                → stream in dashboard
```

### Wyze cameras (post-Thingino)
```yaml
# configuration.yaml (or via UI — Settings → Integrations → Generic Camera)
camera:
  - platform: generic
    name: Wyze Garage Interior
    still_image_url: http://[camera-ip]/cgi-bin/currentpic.cgi
    stream_source: rtsp://[camera-ip]:554/ch0
    verify_ssl: false
```
RTSP URL format: `rtsp://[camera-ip]:554/ch0`

### Automation ideas
- **Leak sensor + camera:** when `binary_sensor.[room]_leak` → on, show camera feed tile on dashboard and send notification with snapshot
- **Alarm armed away + motion:** notify if interior Wyze camera detects motion while Alarmo is armed away
- **Garage cam on dashboard:** always-visible tile on security tab showing garage interior

---

## Open Questions

- [ ] Exact Wyze v2 and v3 unit count — determines how many indoor locations can be covered
- [ ] Sun exposure at proposed Ring outdoor cam downspout locations — walk exterior before mounting
- [ ] Back door / mudroom: is there a power outlet for Wyze v3?
- [ ] Side yard coverage: is it a priority? If so, which cam covers it?

---

## Installation Order

1. Flash all Wyze units with Thingino first (micro-SD method) — do all at once on a table
2. Install Ring Doorbell 2K + transformer
3. Place Ring Indoor Cam (already in garage — confirm it's still the right spot)
4. Walk exterior for sun exposure — confirm Ring outdoor cam locations
5. Mount Ring outdoor cams on downspout brackets
6. Place Wyze cameras at assigned indoor locations
7. Add all streams to HA dashboard security tab

---

*Last updated: 2026-05-25*
*Status: In progress — 2× Ring indoor cams active in HA; 1× Wyze v3 in HA (placement TBD); all 3× Wyze v2 units trashed (image quality); Ring outdoor cams pending sun exposure walk and mount*
*See also: `projects/ring_deployment.md`, `projects/wyze_cameras.md`*
