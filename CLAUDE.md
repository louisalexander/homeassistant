# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## What This Repo Is

A local working copy of Home Assistant configuration files. The live HA instance runs on HAOS at `192.168.4.93` (also reachable via Nabu Casa — see memory for URL). Files are edited here, then pushed to HAOS manually. There is no build system, CI, or test runner.

## Pushing Changes to HAOS

### Automations (most common)
```bash
scp automations.yaml root@192.168.4.93:/config/automations.yaml
# Then in HA: Developer Tools → YAML → Reload Automations
# (No restart needed — automation reload is hot)
```

### Dashboard (Lovelace)
```bash
# Always pull fresh before editing to avoid stale state
ssh root@192.168.4.93 "cat /config/.storage/lovelace.home_command_dashboard" > /tmp/lovelace_dashboard.json

# Edit /tmp/lovelace_dashboard.json (usually via a Python script in /tmp/)
scp /tmp/lovelace_dashboard.json root@192.168.4.93:/config/.storage/lovelace.home_command_dashboard
ssh root@192.168.4.93 "ha core restart"
# Race condition: HA may overwrite the file on shutdown. Always restart immediately after SCP.
```

### Other YAML config files
```bash
scp <file>.yaml root@192.168.4.93:/config/<file>.yaml
ssh root@192.168.4.93 "ha core restart"
```

### Verify HA is up after restart
```bash
ssh root@192.168.4.93 "ha core logs --lines 20"
```

## File Roles

| File | Role | How to apply |
|---|---|---|
| `automations.yaml` | **Single source of truth** for all automations | SCP → Reload Automations |
| `configuration_tou_addition.yaml` | TOU rate/period template sensors + `input_boolean.peak_mode` — loaded via `homeassistant.packages.tou` in `configuration.yaml` | SCP to `/config/`, restart |
| `influxdb_ha.yaml` | InfluxDB domain/entity include-exclude list | SCP to `/config/`, restart |
| `homekit.yaml` | HomeKit bridge entity whitelist | SCP to `/config/`, restart |
| `scripts.yaml` | Alexa-invocable garage scripts | SCP to `/config/`, restart |
| `dashboard.yaml` | **Reference copy only** — NOT the live dashboard | Do not push this file |

**Never edit individual `automation_*.yaml` source files** — these are archived fragments. All active automations live exclusively in `automations.yaml`.

## Architecture

### Automations (automations.yaml)
26 automations in 9 sections. Section header comments in the file define the groups:
1. Leak Detection — critical push notifications (bypasses DND) naming the exact room
2. Garage Doors — open/close events, away notifications, action buttons in notifications
3. Salt Lamp — schedule-based
4. Air Quality — Bedroom — CO₂/VOC threshold alerts
5. Air Quality — Office — CO₂/VOC threshold alerts
6. Battery Alerts — low battery notifications across all ZHA devices
7. Thermostat Presence — away/home preset transitions using `zone.home` (person count, not individual entities)
8. Garage Open Too Long — 30-minute alert with actionable "Close It" notification button
9. TOU Peak Mode — toggles `input_boolean.peak_mode` when `sensor.tou_period` changes

### TOU Sensors (configuration_tou_addition.yaml)
Two template sensors updated every minute:
- `sensor.tou_period` → `peak` / `off_peak` / `super_off_peak` (Dominion Energy Schedule 1G)
- `sensor.tou_rate` → current rate in $/kWh

Season: months 5–9 = summer. Peak hours: summer 3–6pm weekdays, winter 6–9am + 5–8pm weekdays.

### Climate System
- Two Ecobee thermostats: `climate.bedroom` (master bed), `climate.office`
- Both run in `heat_cool` mode (dual setpoint)
- **Ecobee Eco+ TOU is the thermal management system** — HA does not write HVAC setpoints
- Thermostat presence automations use `zone.home` count, not individual person entities
- HA cannot detect hold state directly; hold is inferred from setpoint deviation (see `projects/climate_system.md`)
- Full climate system doc: `projects/climate_system.md`

### Dashboard (Lovelace)
- Dashboard name: **Home Command Dashboard**
- Stored as JSON at `/config/.storage/lovelace.home_command_dashboard` on HAOS
- `dashboard.yaml` in this repo is a reference copy — the `.storage` file is authoritative
- Dashboard edits are always done by pulling the JSON, modifying via Python script in `/tmp/`, and pushing back
- Custom components in use: `custom:button-card`, `custom:mushroom-climate-card`, `custom:mushroom-title-card`, `custom:apexcharts-card`
- Climate tab `button-card` tiles use `[[[JavaScript]]]` template syntax for dynamic color/label/icon

### Integrations & Protocols
- **Ecobee** — native HA integration; also paired directly to Apple Home (not via HA bridge)
- **ZHA** — Zigbee coordinator (ZBT-2 dongle, channel 25); ~15 devices; see Zigbee notes below
- **Z-Wave** — dongle installed, lightly used; available for Z-Wave devices (e.g. smoke detectors)
- **ESPHome** — garage door openers
- **Ring** — cameras + doorbell (for `last_activity` triggers; cameras not bridged to HomeKit)
- **Sonos** — multi-room audio; entities `media_player.master_bedroom_sonos_one`, `media_player.sonos_play_5`; use for TTS announcements
- **Mobile App** — `notify.mobile_app_louis_iphone` and `notify.mobile_app_lindsays_iphone`
- **InfluxDB 1.x** — time-series storage; Grafana uses **InfluxQL** (not Flux)
- **HomeKit Bridge** — whitelist-only; 2 garage doors + 10 leak sensors + 4 plugs
- **Alexa** — primary voice interface; multiple Echo devices throughout house
- **Apple Home** — secondary; Ecobee paired natively, other devices via HA HomeKit Bridge

### Network Infrastructure
- **Gateway:** Eero Pro mesh (1 Gbps fiber)
- **Switch:** TP-Link TL-SG116 (16-port unmanaged) in SMC
- **Backbone:** Mix of Cat6 and MoCA (ScreenBeam adapters + quad-shield RG6 on coax runs between floors)
- **Patch panel:** VCELINK 24-port Cat6A keystone in SMC
- **SMC contents:** Mini S13 PC, MoCA adapter, TP-Link switch
- **Zigbee coordinator:** ZBT-2 USB dongle on Sabrent USB 2.0 hub + extension cable (kept away from Eero to avoid 2.4 GHz interference); channel 25

### Key Entities
- `zone.home` — person count in the Home zone (use this for presence, not individual person entities)
- `person.louis_alexander`, `person.lindsay` — the two residents (`person.lindsay_saady` does NOT exist)
- `input_boolean.peak_mode` — true during TOU peak hours
- `sensor.tou_period` — current TOU period string
- `weather.forecast_home` — used for outside temperature in thermostat conditions

### Known Quirks
- **Garage door entity naming:** `cover.left_garage_door_opener_garage_door` is actually the *right* door in physical terms — naming mismatch from installer. Labels in UI reflect physical reality; entity IDs are inverted.
- **Zigbee sensor log heartbeats:** Third Reality leak sensors report state every ~60s via ZHA polling (IasZone cluster doesn't support interval config). Normal behavior — filter from logbook if noisy.
- **Dashboard race condition:** HA overwrites `.storage/lovelace.*` on shutdown. Always `ha core restart` immediately after SCP to minimize the window.

### Zigbee Best Practices
1. Pair mains-powered devices (switches, plugs) **before** battery sensors — they form the mesh backbone
2. Wait **24 hours** after adding powered devices before testing signal — neighbor tables rebuild gradually
3. Channel 25 chosen deliberately to avoid 2.4 GHz WiFi overlap
4. ZHA coordinator migration: use the ZHA radio migration tool — no re-pairing needed
5. Sleeping end devices rejoin automatically after coordinator restarts via channel scan

## Project Documentation
Detailed design docs for ongoing projects live in `projects/`:
- `projects/climate_system.md` — climate cards, mode indicator, TOU detection, automations reference
- `projects/house_energy_strategy.md` — EM16P energy monitoring project
- `projects/fan_smart_control.md` — Hunter fan (superseded — merged into Inovelli Batch 6)
- `projects/inovelli_lighting.md` — whole-house Inovelli Blue Series Zigbee switch rollout
- `projects/alarm_system.md` — Konnected Alarm Panel Pro install (purchased, not yet installed)
- `projects/smoke_detectors.md` — whole-home smoke/CO detector replacement project
- `projects/smart_locks.md` — smart lock consideration (Schlage, model TBD)
- `projects/camera_plan.md` — Unified camera strategy: Ring (perimeter/cloud) + Wyze (interior/local); placement assignments for all cameras
- `projects/ring_deployment.md` — Ring doorbell upgrade, outdoor cameras, chime distribution (hardware detail)
- `projects/wyze_cameras.md` — Wyze v2/v3 fleet flash with Thingino → local RTSP → HA (hardware detail)
- `projects/gosund_sl2.md` — Gosund SL2 LED strip (on hold, device not located)
- `projects/fireplace_automation.md` — Gas fireplace on/off + blower speed automation (investigation phase)

**Keep these docs current** whenever the systems they document change.

## Hardware On Hand — Not Yet Integrated

| Device | Qty | Project | Status |
|---|---|---|---|
| 1st Floor Ecobee Premium | 1 | `climate_system.md` | Purchased, not installed |
| Konnected Alarm Panel Pro | 1 | `alarm_system.md` | Purchased, not installed |
| Ring Alarm Gen 2 Base Station | 1 | `alarm_system.md` | To order — bridges Alarmo → Ring Pro monitoring via ring-mqtt |
| First Alert Z-Wave ZCOMBO-G | TBD | `smoke_detectors.md` | Not yet ordered — walk house to count locations first |
| Schlage BE469ZP (Z-Wave lock) | TBD | `smart_locks.md` | Not yet ordered — front door first |
| Inovelli VZW31-SN | TBD | `inovelli_lighting.md` | Batch 1 (4× exterior) ready to order |
| Inovelli VZW36 Fan+Light | 3 | `inovelli_lighting.md` | Not yet ordered |
| Inovelli VZM35-SN Canopy Module | 3 | `inovelli_lighting.md` | Not yet ordered |
| Ring Indoor Cam (plug-in) | 1 | `ring_deployment.md` | Not placed |
| Ring Outdoor Cam + Solar Panel | 2 | `ring_deployment.md` | Not mounted — downspout location TBD |
| Ring Video Doorbell Powered 2K | 1 | `ring_deployment.md` | Not installed |
| Ring Doorbell Transformer | 1 | `ring_deployment.md` | In hand — not yet installed; pairs with Powered 2K |
| Ring Chime V3 | 4 | `ring_deployment.md` | Not placed |
| Ring Chime (original) | 1 | `ring_deployment.md` | Not placed — check if already registered |
| Ring Chime Pro (original) | 1 | `ring_deployment.md` | Not placed — check if already registered |
| Wyze Cam v2 | 2+ | `wyze_cameras.md` | Count TBD; not flashed |
| Wyze Cam v3 | 1+ | `wyze_cameras.md` | Count TBD; not flashed |
| Leak Detector (ZHA) | 2 | No project needed | Pair to ZHA; existing leak automations pick them up automatically |
| Gosund SL2 LED Strip | 1 | `gosund_sl2.md` | On hold — can't locate device; likely ESP/Tasmota-flashable (see project file) |
| Govee Floor Lamp 2 | 1 | No project yet | On hold — power plug misplaced; govee2mqtt path when found |
