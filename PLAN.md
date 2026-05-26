# Smart Home Master Execution Plan

*Last updated: 2026-05-26 — update this file whenever any project advances.*

---

## Executive Summary

10 active projects across security, energy, lighting, cameras, and climate. Three hardware items already in the house are blocking multiple project chains — install the 1st floor Ecobee, EM16P monitors (overdue since May 22), and Konnected Alarm Panel Pro. Ring Chime V3 units are now active in HA as siren entities. Wyze v2 cameras trashed (poor image quality); v3 remains. Two projects (Gosund SL2, Govee Lamp) are on hold pending locating the hardware.

**Ordering policy:** Don't order new hardware while uninstalled inventory sits on hand, unless the order is a hard blocker for an install step (not just project completion). Goal: minimize outstanding inventory and overspend.

**Top 3 priorities right now:**
1. **EM16P install** — **overdue** (was May 22); still blocks all energy monitoring and WS0 baseline setup
2. **Inovelli outdoor switches** — batch arrived 2026-05-26; 5 single-pole installs first (Front Hanging Light, Flood Lights, Patio Door, High Flood, Personnel Door exterior), no aux complexity; unlocks indoor batch ordering
3. **Install 1st floor Ecobee** — hardware in hand, one crawlspace trip (connect blue wire to York C terminal), largest single ROI improvement

---

## Project Status Dashboard

| Project | Status | Next Action | Blocked By |
|---|---|---|---|
| House Energy — EM16P | **Active** | Install monitors — **OVERDUE** (was May 22) | Scheduled |
| Climate System — 1st floor Ecobee | **Ready — hw in hand** | Crawlspace trip: connect blue wire to York C terminal, then install | Nothing |
| Camera Plan (Ring + Wyze) | **In Progress** | Mount 2× Ring outdoor solar cams (sun walk first); place 1× Wyze v3 | Sun exposure walk |
| Alarm System (Konnected) | **Ready — hw in hand** | Wire 4 zones + siren; configure Alarmo | Nothing |
| Inovelli Lighting — Outdoor Batch | **In hand — ready to install** | 5 single-pole first (no aux), then 2 three-way | Nothing |
| Smoke Detectors | **Ready to order** | Order 9× ZCOMBO-G when ready to install same week | Ordering policy |
| Inovelli Lighting — Indoor Batches | **Ready to order** | Order Batch 2 (Master Bedroom) after outdoor installed | Ordering policy |
| Smart Locks | **Hold** | Order Schlage BE469ZP after hardware backlog clears | Inventory policy |
| Gas Fireplace Automation | **Deferred** | Revisit fall/winter — millivolt confirmed; need blower wattage | Seasonal |
| Gosund SL2 | **On Hold** | Locate device | Can't find hardware |
| Govee Floor Lamp | **On Hold** | Locate power plug | Plug missing |
| Fan Smart Control | **Closed** | Merged into Inovelli Batch 6 | — |

---

## Dependency Map

```
EM16P Install (May 22)
  └─► Energy WS0 (entity setup)
        └─► Energy WS1 (30-day baseline monitoring)

Smoke Detector Order (9× ZCOMBO-G — order when ready to install)
  └─► Smoke Detector Install
        └─► Konnected Alarm Install
              └─► Alarmo Configuration
                    └─► Ring Alarm Base Station Order + ring-mqtt setup

Inovelli Outdoor Install (batch arriving soon)
  └─► Inovelli Indoor Batch 2 Order
        └─► Inovelli Indoor Batches 3–10 (order one at a time)

No dependencies (order/install any time):
  - 1st Floor Ecobee
  - Inovelli Batch 1 (4 exterior switches — already verified)
  - Smart Lock (front door)
  - Ring Doorbell 2K + transformer
  - Ring Chimes (place and register)
  - Wyze v3 placement (v2 units trashed; v3 only)
```

---

## Phased Execution Plan

### This Week (May 21–25)

- [ ] **May 22 (OVERDUE)** — Install 2× EM16P energy monitors (both panels)
- [ ] **May 22–23 (BLOCKED)** — Run Energy WS0 in HA: discover entities, confirm statistics, build whole-home template sensors
- [x] **Walk the house** — Smoke detector count (9 confirmed, all 3-wire); Inovelli survey complete (29/30 verified)
- [x] **Order Inovelli outdoor batch** — 10× VZW31-SN + 5× VZW05 ordered (all 7 outdoor locations + extras)
- [x] **Inovelli outdoor batch arrived** 2026-05-26 — ready to install
- [x] **Check Ring account** — Chime v1 + Chime Pro removed from account 2026-05-21; 4× V3 chimes registered and active in HA as siren entities

### Next 30 Days (May 25 – Jun 21)

- [ ] **Install 1st floor Ecobee** — connect blue wire to York furnace C terminal (crawlspace); add thermostat card + temp tile to dashboard immediately after
- [ ] **Install Ring Doorbell Powered 2K + transformer** — both in hand; replace existing transformer, install doorbell
- [x] **Distribute 4× Ring Chime V3** — 1st floor kitchen, 1st floor foyer, 2nd floor hall, 3rd floor office
- [ ] **Mount 2× Ring outdoor cameras** — downspout brackets in hand; confirm sun exposure + coverage zone first
- [x] **Install Ring indoor cam** — Cam 1: garage; Cam 2: personnel door (house side) — both active in HA
- [x] **Flash Wyze cameras** — 3× v2 flashed (Thingino) but trashed 2026-05-25 (poor image quality); 1× v3 flashed, in HA — placement TBD
- [ ] **Install Inovelli outdoor switches** — batch in hand; 5 single-pole first, then 2 three-way locations
- [ ] **Install Konnected Alarm Panel Pro** — wire 4 zones + siren; configure Alarmo; Sonos TTS announcements
- [ ] **Install smoke detectors** — order 9× ZCOMBO-G when ready to install same week; replace all at once
- [ ] **Energy WS1** — Begin 30-day baseline monitoring (passive, no action needed after WS0 setup)

### 30–90 Days (Jun 21 – Aug 21)

- [x] **Resolve Noonlight vs DIY** — Ring Protect Pro purchased; Ring Alarm Base Station + ring-mqtt is the monitoring path (no separate Noonlight subscription)
- [ ] **Order + install Schlage BE469ZP** — front door; order when hardware backlog is clear
- [ ] **Order Ring Alarm Gen 2 Base Station** — order after Konnected is commissioned; completes Ring Pro monitoring integration
- [ ] **Inovelli indoor batches** — Master Bedroom, Office, Family Room, Kitchen, etc. — order one batch at a time
- [ ] **Energy WS1 review** — 30-day baseline complete; identify phantom loads; act on any significant findings
- [ ] **Smart lock expansion** — decide additional doors after front door is validated

### Deferred / When Triggered

| Item | Trigger to Activate |
|---|---|
| Inovelli Batches 6–8+ | Survey complete; earlier batches installed and working |
| Energy WS2–WS5 (financial reporting, advanced TOU) | High-value controllable load added (EV, solar, battery) |
| Gas Fireplace Automation | Fall/winter — confirmed millivolt switch; need blower motor wattage before ordering |
| Gosund SL2 | Device located |
| Govee Floor Lamp | Power plug located |
| Frigate NVR (AI camera detection) | Current camera setup insufficient; Coral TPU available |
| Additional smart locks | Front door workflow validated; clear use case per door |
| Alarmo → Noonlight | Professional monitoring decision made |

---

## Per-Project Action Tracker

### Climate System
- [x] Dashboard mode cards + temp deviation tiles live
- [x] Ecobee Eco+ TOU integration documented
- [ ] Install 1st floor Ecobee Premium
- [ ] Add 1st floor thermostat card + temp tile to dashboard (do immediately after install)
- [ ] VOC investigation (test cooking hypothesis once 1st floor sensor is live)

### House Energy Strategy
- [x] TOU sensors (period + rate) live
- [x] `input_boolean.peak_mode` live
- [ ] Install EM16P monitors (May 22)
- [ ] WS0: Discover entities, confirm statistics, build whole-home template sensors
- [ ] WS1: 30-day baseline monitoring (passive after WS0)
- [ ] WS1 review: identify phantom loads, act on findings
- [ ] *Defer WS2–WS5 until high-value load (EV/solar) justifies the instrumentation*

### Smoke Detectors
- [x] Count detector locations — **9 confirmed**
- [x] 3-wire interconnect confirmed at all 9 locations
- [ ] Order 9× First Alert Z-Wave ZCOMBO-G (~$405–495)
- [ ] Replace all detectors simultaneously
- [ ] Pair to Z-Wave in HA; add to Alarmo triggers

### Inovelli Lighting
- [x] Survey complete — all 30 locations verified (Family Room Fan wiring still needs physical confirmation before ordering)
- [x] Protocol confirmed: ZHA (not Z2M)
- [x] Shopping totals confirmed: 29× VZW31-SN, 3× VZW36, 3× VZM35-SN, 18× VZW05
- [x] Order outdoor batch: 10× VZW31-SN + 5× VZW05 ordered
- [x] Outdoor batch arrived 2026-05-26
- [ ] **Install outdoor switches — batch in hand**; 5 single-pole first (Front Hanging Light, Flood Lights, Patio Door, High Flood, Personnel Door), then 2 three-way (Outside Garage Lights, Garage Lights)
- [ ] Order indoor Batch 2 (Master Bedroom can lights: 1× VZW31-SN dimmer) after outdoor install complete
- [ ] Family Room Fan (Batch 6): confirm neutral at wall box + canopy before ordering VZW36 + VZM35-SN

### Alarm System (Konnected + Ring Alarm base station)
- [x] Konnected Alarm Panel Pro purchased
- [x] GE Concord Express 60-806 existing panel documented (4 zones)
- [x] Ring Protect Pro active — professional monitoring funded
- [x] Noonlight decision resolved — Ring base station + ring-mqtt bridges Alarmo → Ring Pro monitoring; no separate Noonlight subscription needed
- [x] **Mock door automations live** — Section 12 in automations.yaml: 3 input_button helpers (front/garage/rear door test) + 3 automations that ring all 4 chimes with ding tone on button press; replace with Konnected binary_sensor entities when wired
- [ ] **Order Ring Alarm Gen 2 Base Station** (~$60–100) — unlocks Ring Pro monitoring for Alarmo
- [ ] Install ring-mqtt HACS add-on in HA
- [ ] *Install smoke detectors before Konnected*
- [ ] Wire Konnected to 4 zones + siren
- [ ] Configure Alarmo in HA using Konnected sensor entities
- [ ] Pair Ring base station → ring-mqtt → confirm panic switches appear as HA entities
- [ ] Automation: Alarmo triggered → `switch.ring_alarm_police_panic`
- [ ] Automation: smoke detector triggered → `switch.ring_alarm_fire_panic`
- [ ] Set up Sonos TTS announcements for alarm events

### Smart Locks
- [ ] Order 1× Schlage BE469ZP (front door)
- [ ] Install + pair to Z-Wave JS
- [ ] Configure access codes via Z-Wave JS UI
- [ ] Validate Alarmo integration (arm away if locked, notify if armed while unlocked)
- [ ] Decide remaining doors after workflow validated

### Ring Deployment
- [x] Ring transformer in hand (no need to order)
- [x] Ring Chime v1 + Chime Pro — removed from Ring account 2026-05-21
- [x] Ring Protect Pro — 1-year subscription purchased 2026-05-21 (renews ~2027-05-21)
- [x] Remove deprecated chimes from Ring app — completed 2026-05-21
- [x] 4× Ring Chime V3 active in HA as siren entities (siren.[name], volume controls available)
- [ ] Install Ring Doorbell Powered 2K + transformer (both in hand)
- [x] Physically distribute and mount 4× Ring Chime V3s (1st floor kitchen, 1st floor foyer, 2nd floor hall, 3rd floor office) — placed 2026-05-26
- [ ] Mount 2× outdoor solar cameras — confirm downspout sun exposure first
- [x] Place indoor cams — Cam 1: garage, Cam 2: personnel door (house side) — both active in HA
- [x] Verify Ring devices in HA Ring integration — both indoor cams + chimes appear

### Wyze Cameras
- [x] Count exact unit inventory — 3× v2, 1× v3
- [x] Flash all units with Thingino firmware (wltechblog installer, micro-SD)
- [x] Enable RTSP on each camera (Thingino built-in)
- [x] Add to HA as Generic Camera integration
- [x] All 3× v2 units trashed 2026-05-25 (poor image quality); dashboard cards removed; v3 remains
- [x] v3 camera card live on dashboard (full-width picture-entity card)
- [x] Place v3 — deployed in Office (3rd floor) 2026-05-26; dashboard tile renamed "Office Cam"

---

## Hardware Waiting to Order

*Policy: don't order while uninstalled inventory sits on hand. Only order if it hard-blocks an install step.*

| Item | Qty | Order Trigger | Est. Cost |
|---|---|---|---|
| Inovelli VZW31-SN (outdoor batch) | 10 | **Arrived 2026-05-26** — 7 outdoor + 3 carry to indoor | ~$600 |
| Inovelli VZW05 (aux, outdoor batch) | 5 | **Arrived 2026-05-26** — 2 outdoor + 3 carry to indoor | ~$125 |
| Downspout mount brackets (Ring) | 2 | **In hand** | ~$15 ea |
| First Alert Z-Wave ZCOMBO-G | 9 | Order when ready to install (not before) | ~$45–55 ea (~$405–495 total) |
| Schlage BE469ZP | 1 | Order when existing hardware backlog is cleared | ~$160 |
| Ring Alarm Gen 2 Base Station | 1 | Order after Konnected is installed and commissioned | ~$60–100 |
| Inovelli VZW31-SN (indoor batches) | 22 remaining | Order one batch at a time as each is installed | ~$60 ea |
| Inovelli VZW36 + VZM35-SN (fans) | 3 ea | Order when fan batch is next up | ~$110/pair |
| Inovelli VZW05 (indoor batches) | 13 remaining | Order with each indoor batch | ~$25 ea |

---

*Keep this file current: update status checkboxes and table entries whenever a project advances.*
*Full project details live in `projects/*.md` — this file tracks execution state, not design decisions.*
