# Inovelli Lighting Plan

## House Context
4,800 sq ft · 3 floors · Built 2006 · Zigbee via **ZHA** (ZBT-2 dongle, channel 25)

> **Protocol note:** Inovelli Blue Series is **Zigbee**, not Thread/Matter. ZHA is the chosen integration — LED notifications, multi-tap events, and parameter config all work via ZHA. Z2M is installed but not running; no migration planned.

---

## Current Shopping List Status

### What's Planned (from spreadsheet)

| Room | Group | Switch Type | Aux/Dumb | Verified |
|---|---|---|---|---|
| Office | Can Lights | 1 Dimmer | 1 | ✓ |
| Office | Corridor | 1 Dimmer | 1 | ✓ |
| Office | Fan | 1× VZW31-SN (dimmer, light) + 1× VZW36 (fan) | — | ✓ |
| Kitchen | Can Lights | 1 Dimmer | 3 | ✓ |
| Kitchen | Butler's Kitchen | 1 Dimmer | 1 | ✓ |
| Kitchen | Kitchenette | 1 Dimmer | — | ✓ |
| Laundry Room | Can Lights | 1 Dimmer | 1 | ✓ |
| Family Room | Can Lights | 1 Dimmer | 1 | ✓ |
| Family Room | Fan | 1× VZW36 + 1× VZM35-SN canopy | — | — (wiring TBD) |
| Family Room | Switched Outlet | 1 On/Off | 1 | ✓ |
| Master Bedroom | Can Lights | 1 Dimmer | — | ✓ |
| Master Bedroom | Fan | 1× VZW31-SN (dimmer, light) + 1× VZW36 (fan) | — | ✓ |
| Master Bedroom | Bathroom Hallway | 1 Dimmer | — | ✓ |
| Master Bedroom | Switched Outlet | 1 On/Off | — | ✓ |
| Upstairs Hallway | — | 1 On/Off | 4 | ✓ |
| Exterior | Front Hanging Light | 1 On/Off | — | ✓ |
| Exterior | Flood Lights | 1 On/Off | — | ✓ |
| Exterior | Outside Garage Lights | 1 On/Off | 1 | ✓ |
| Exterior | Personnel Door | 1 On/Off | — | ✓ |
| Exterior | Patio Door | 1 On/Off | — | ✓ |
| Exterior | High Flood | 1 On/Off | — | ✓ |
| Garage | Garage Lights | 1 On/Off | 1 | ✓ |
| Lindsay's Office | Can Lights 1 | 1 Dimmer | — | ✓ |
| Lindsay's Office | Can Lights 2 | 1 Dimmer | — | ✓ |
| Lindsay's Office | Switched Outlet | 1 On/Off | — | ✓ |
| 3rd Floor Stairs | Hallway Light | 1 On/Off | 1 | ✓ |
| Foyer | Big Light | 1 Dimmer | 1 | ✓ |
| Foyer | Can Lights | 1 Dimmer | — | ✓ |
| Front Room | Switched Outlet | 1 On/Off | — | ✓ |
| Dining Room | Chandelier | 1 Dimmer | 1 | ✓ |

### Shopping Totals (what's currently planned)

| Switch Type | Count | Product |
|---|---|---|
| Smart Dimmer | 16 | VZW31-SN Blue Series 2-1 (dimmer mode) |
| Smart On/Off | 13 | VZW31-SN Blue Series 2-1 (on/off mode) |
| Fan Controller | 3 | VZW36 Blue Series Fan+Light |
| Fan Canopy Module | 3 | VZM35-SN (pairs with VZW36; one per fan: Office, Master Bedroom, Family Room) |
| Aux/Dumb Switch | 18 | VZW05 or compatible dumb switch |
| Presence sensing | —  | Standalone Zigbee mmWave sensor (Sonoff SNZB-06P recommended) — see design note below |

> **VZW31-SN total: 29** (16 dimmers + 13 on/off). Same SKU for both — mode is firmware-configured.
>
> **VZM35-SN note:** All 3 fan locations (Office, Master Bedroom, Family Room) need 1× VZM35-SN at the fan canopy. For Office and Master Bedroom: verify whether the existing fan has a canopy receiver module that the VZM35-SN replaces, or if the fan was always controlled via pull chain (requiring new wiring at the canopy).

> **Inovelli mmWave Presence Dimmer exists** (Blue Series, ZHA-supported) but is not used here — standalone sensors give better FOV placement flexibility. Exception: check switch box orientation per-room; if the box faces the right direction, the built-in option eliminates a device.

---

## Known Gaps in the Current Plan

### 1. Family Room Fan — resolved
Assigned: VZW36 + VZM35-SN canopy module, same approach as office and master bedroom fans. RF diagnostic path (see `fan_smart_control.md`) was evaluated and dropped — canopy replacement is cleaner and the hardware cost delta doesn't justify the complexity. Verify neutral wire at the wall box and hot+neutral at the canopy before ordering.

### 2. Presence Detection — standalone sensors, not switch-mounted
The spreadsheet has a "Presence Dimmer" column but nothing is assigned. **Design decision: use a standard VZW31-SN at the switch box + a separately mounted Zigbee presence sensor.** Reasons:
- Switch box location rarely corresponds to the optimal sensor field of view. E.g. Louis' office switch is near the door but the desk is behind a large monitor — a wall-mounted sensor pointing into the room from the opposite wall detects seated presence that a switch-mounted sensor would miss entirely.
- Standalone sensors can be ceiling-mounted, corner-mounted, or aimed precisely at a work zone.

**Sensor type:** Use **mmWave radar** sensors for occupied-but-still scenarios (desk work, reading, watching TV). PIR sensors require motion to trigger — they'll turn the lights off while someone is sitting still.

**Recommended Zigbee mmWave sensors (ZHA-compatible, no brand hub required):**
- **Sonoff SNZB-06P** — best default choice; Zigbee mmWave, ZHA supported, ~$15, pairs like any other Zigbee device
- **Tuya ZY-M100 / TS0601 variants** (Moes, TUYATEC, etc.) — cheap (~$12–18), ZHA supported, quality varies by batch; buy one to test before ordering in quantity
- **Linptech ES1ZZ** — Zigbee mmWave, ZHA supported, slightly more tunable detection zones

**Avoid:** Aqara FP2 (Matter/WiFi, needs Aqara hub), Aqara FP1 (Zigbee but Aqara pushes hub dependency and ZHA support is inconsistent across firmware versions), IKEA Vallhorn (PIR only — won't detect seated still presence)

**Presence automation pattern in HA:**
```
Trigger: presence sensor → detected
Action: turn on light

Trigger: presence sensor → clear (after N min timeout)
Action: turn off light
```
Manual switch overrides always work — switch press bypasses the automation state.

**Likely presence sensor locations to plan:**
- Louis' Office (sensor on far wall pointing at desk)
- Bathrooms (on when occupied, off 5 min after clear)
- Laundry Room
- Garage interior
- Hallways (shorter timeout — 2 min)

### 3. Rooms not in the plan

**Explicitly excluded (user decision):** Bedrooms 1–4 (kids' rooms) — no smart switches planned.

**Still missing — not yet in spreadsheet:**

| Room | Likely Switch Need | Notes |
|---|---|---|
| Master Bathroom | On/Off or Dimmer | Vanity lights, exhaust fan |
| 2nd Floor Hall Bathroom | On/Off or Dimmer | Leak sensor exists — room is real |
| Other Bathrooms | On/Off | — |
| 1st Floor Hallway | On/Off or Dimmer | If separate from Foyer path |
| 1st → 2nd Floor Stairs | On/Off | 3rd floor stairs is in plan; 1st-2nd is not |

### 4. Lindsay's Office — On/Off only
Currently has only a single On/Off switch with no subgroup. Likely needs the same treatment as Louis' Office (can lights dimmer + corridor dimmer + fan). Confirm what fixtures are in the room.

### 5. Aux switch counts — field verification remaining
- **Kitchen Can Lights: 3 aux** — implies a 4-way configuration (or two separate 3-way runs). Confirm layout before ordering Kitchen batch.
- **Upstairs Hallway: 4 aux** — ✓ confirmed 5 total switch positions (1 smart + 4 aux).
- **Foyer Big Light: 1 aux** — corrected from earlier estimate of 2; confirmed by spreadsheet.

---

## Open Questions to Answer Before Purchasing

- [ ] **Family Room Fan** — confirm neutral wire at wall switch box and hot+neutral at canopy before ordering Batch 6 hardware. Confirm Hunter RF receiver bypass approach.
- [ ] **Office + Master Bedroom fans (VZM35-SN)** — do existing fans have an aftermarket canopy receiver module (typical in remotely-controlled fans), or are they pull-chain only? If pull-chain: new wiring needed at the canopy. If receiver exists: VZM35-SN replaces it directly.
- [ ] **Kitchen Can Lights aux config** — 3 aux switches implies a 4-way (or two 3-way runs). Confirm physical switch count and layout before ordering Batch 7.
- [ ] **Presence locations** — which rooms should auto-on/off? Bathrooms? Laundry?
- [ ] **Bathrooms** — are vanity lights dimmable? Want smart switches in bathrooms at all?
- [ ] **Neutral wire availability** — verify any location where it's uncertain before ordering that batch (2006 build likely has neutral everywhere but older wiring runs may not).

---

## Recommended Project Phases

### Phase 1 — Survey — Complete
All 30 planned locations are verified in the spreadsheet. Family Room Fan wiring still needs physical confirmation before ordering Batch 6 hardware. Kitchen Can Lights 3-aux layout needs confirmation before ordering Batch 7.

### Phase 2 — Resolve Remaining Design Questions
1. Confirm Family Room Fan wiring (neutral at wall box, hot+neutral at canopy, Hunter RF bypass approach)
2. Confirm Office + Master Bedroom fan canopy wiring type (receiver module or pull-chain)
3. Confirm Kitchen Can Lights switch layout (4-way or two 3-way runs)

### Phase 3 — Order in Batches by Room Group
Don't order everything at once. Order one room group, install, confirm it works end-to-end in ZHA + HA, then order the next.

**Rationale for exterior-first:** Immediate security + convenience ROI (sunset scheduling, arrival automations, Ring integration). All exterior locations are on/off only — no dimmer curve configuration or LED compatibility concerns.

**Caveat:** Two exterior locations (Outside Garage Lights, Personnel Door) have aux switches. Do the simple exterior locations first, learn 3-way wiring on a less exposed indoor location, then return for those two.

| Batch | Locations | Hardware | Rationale |
|---|---|---|---|
| **1 — Simple Exterior** | Front Hanging Light, Flood Lights, Patio Door, High Flood | 4× VZW31-SN | Highest ROI, zero aux complexity, prove ZHA workflow |
| **2 — First Indoor Dimmer** | Master Bedroom Can Lights | 1× VZW31-SN | Learn dimmer config + LED compatibility on verified single-pole location |
| **3 — Exterior w/ Aux** | Outside Garage Lights, Personnel Door | 2× VZW31-SN, 1× VZW05 | Learn 3-way wiring before tackling indoor runs |
| **4 — Office** | Can Lights, Corridor, Fan | 3× VZW31-SN, 1× VZW36, 2× VZW05, 1× VZM35-SN | All verified; add presence sensor; Fan = double-gang VZW31-SN + VZW36 |
| **5 — Master Bedroom** | Fan, Bathroom Hallway, Switched Outlet | 3× VZW31-SN, 1× VZW36, 1× VZM35-SN | All verified; Fan = double-gang VZW31-SN + VZW36 |
| **6 — Family Room** | Can Lights, Fan, Switched Outlet | 2× VZW31-SN, 1× VZW36, 1× VZM35-SN, 2× VZW05 | Confirm neutral at wall box + canopy; Hunter RF receiver bypass |
| **7 — Kitchen + Laundry** | Kitchen Can Lights, Butler's Kitchen, Kitchenette, Laundry Can Lights | 4× VZW31-SN, 5× VZW05 | Kitchen Can Lights has 3 aux — confirm layout before ordering |
| **8 — Foyer + Dining** | Foyer Big Light, Foyer Can Lights, Dining Room Chandelier | 3× VZW31-SN, 2× VZW05 | High-visibility 1st floor; all dimmers |
| **9 — Lindsay's Office** | Can Lights 1, Can Lights 2, Switched Outlet | 3× VZW31-SN | Three circuits, no aux switches |
| **10 — Remaining** | Upstairs Hallway, Garage Lights, 3rd Floor Stairs, Front Room | 4× VZW31-SN, 6× VZW05 | Upstairs Hallway has 4 aux switches (5-position run confirmed) |

### Phase 4 — Install & Commission
For each room:
1. Cut power at breaker
2. Install VZW31-SN or VZW36 at smart switch location
3. Install VZW05 aux switches at all secondary locations in the same run
4. Power on — switch should join ZHA automatically (hold config button if needed)
5. Name entity in ZHA, assign to room in HA
6. Configure parameters (dimmer curve, min/max brightness, LED color) via ZHA "Manage Zigbee Device" or HA
7. Create any HA automations (presence, scene control, multi-tap actions)

### Phase 5 — HA Integration
- All switches join via **ZHA** — full Inovelli Blue Series parameter support confirmed (LED notifications, multi-tap events, parameter config via "Manage Zigbee Device")
- Each switch exposes: light entity, switch entity, LED notification entity, button events
- Multi-tap events come through as HA events — use them for scenes
- LED bar can be used as status indicators (e.g., orange during TOU peak, blue for notifications)
- OTA firmware updates available through ZHA

---

## Product Reference

| Product | Use Case | Notes |
|---|---|---|
| VZW31-SN Blue Series 2-1 | All dimmable lights + on/off locations | Same SKU for both — mode is firmware-configured |
| VZW36 Blue Series Fan+Light | Fan locations with light kit | Wall switch; pairs with VZM35-SN canopy module |
| VZM35-SN Fan+Light Canopy | Inside fan housing | Pairs with VZW36; replaces RF receiver |
| VZW05 Blue Series Aux | Secondary locations in 3-way/4-way runs | Wired to VZW31-SN or VZW36 via traveler |

> **Fan wiring requirement:** VZW36 requires neutral at the wall switch box. VZM35-SN requires hot + neutral at the canopy. Verify both before ordering fan hardware.

---

## HA Entity Naming Convention (proposed)

```
light.office_can_lights
light.office_corridor
fan.office_fan
light.family_room_can_lights
fan.family_room_fan
light.master_bedroom_can_lights
fan.master_bedroom_fan
...
```

Set these names at pairing time in ZHA — much easier than renaming after the fact.

---

## Related Projects
- `projects/fan_smart_control.md` — Hunter ceiling fan RF analysis (superseded — canopy replacement approach adopted; see Batch 6)
- `projects/house_energy_strategy.md` — EM16P energy monitoring; Inovelli switches do not report power consumption so smart plugs remain separate

---

*Last updated: 2026-05-26*
*Status: Outdoor batch arrived 2026-05-26 (10× VZW31-SN + 5× VZW05). Ready to install — 5 single-pole first, then 2 three-way. Indoor batches unlock after outdoor install.*
