# Plant & Garden Dashboard

## Goal

Build a dedicated "Garden" tab on the Home Command Dashboard that Lindsay can use to monitor and care for all indoor and outdoor plants. Integrates indoor soil moisture sensors, outdoor soil moisture sensors, a personal weather station, and smart irrigation (Rachio) into a unified view with automated care reminders keyed to the Yard Care Guide.

---

## Hardware Inventory

| Device | Qty | Status | Location |
|---|---|---|---|
| ThirdReality Smart Soil Moisture Sensor Gen2 (Zigbee) | 3 | **On order** | Indoor — 3× Fiddle Leaf Figs |
| ThirdReality Smart Soil Moisture Sensor (Zigbee) | 2+ | **Deployed in yard** | Outdoor — front yard + rear yard (pair to ZHA) |
| Ecowitt WS90 all-in-one weather station | 1 | **Planned — not yet ordered** | Outdoor pole |
| Rachio 3 (6-zone) | 1 | **In hand — not yet installed** | Replaces Orbit B-Hyve |
| Orbit B-Hyve (6-zone) | 1 | **Current controller** | Remove after Rachio install |

---

## Indoor Plants

| Friendly Name | Location | Sensor | Notes |
|---|---|---|---|
| Maury River Fiddle Leaf | Kitchen | ThirdReality (on order) | FLF — most moisture-sensitive |
| Fiddle Leaf Fig — [Room TBD] | TBD | ThirdReality (on order) | FLF #2 |
| Fiddle Leaf Fig — [Room TBD] | TBD | ThirdReality (on order) | FLF #3 |
| Louis' Monstera | Kitchen | Last-watered tracking | Monstera — forgiving |
| Bird of Paradise | Kitchen | Last-watered tracking | Single specimen |
| Monstera — [Room TBD] | TBD | Last-watered tracking | Monstera #2 |
| Monstera — [Room TBD] | TBD | Last-watered tracking | Monstera #3 |

**Sensor allocation rationale:** FLFs are the most notoriously finicky (punish both over and underwatering). All 3 sensors go there first. Order a second 3-pack when ready — Bird of Paradise and one Monstera are next priority.

**Expected indoor sensor entities (pair via ZHA, then check Developer Tools):**
- `sensor.maury_river_fiddle_leaf_moisture`
- `sensor.fiddle_leaf_fig_2_moisture`
- `sensor.fiddle_leaf_fig_3_moisture`

**Last-watered helpers to create:**
- `input_datetime.last_watered_louismonster`
- `input_datetime.last_watered_birdofparadise`
- `input_datetime.last_watered_monstera_2`
- `input_datetime.last_watered_monstera_3`

---

## Outdoor Irrigation Zones

**Current controller:** Orbit B-Hyve → **replacing with Rachio 3**

| Zone | Name | Area | Key Plants | Soil Sensor |
|---|---|---|---|---|
| 1 | Front | Front lawn/beds | TBD — map during field session | `sensor.soil_moisture_front` |
| 2 | Left Side | Left side yard | TBD | — |
| 3 | Right Side | Right side yard | TBD | — |
| 4 | Rear Left | Rear left | TBD — map during field session | `sensor.soil_moisture_rear` |
| 5 | Rear Right | Rear right | TBD | — |
| 6 | Drip/Garden | Drip / garden beds | TBD | — |

> **Field session required:** Walk each zone while running it one at a time. Use the Yard Map Tool (see below) to record every head position. Fill in "Key Plants" column above after mapping.

**Rachio HA entity pattern:**
- `switch.rachio_zone_1` through `switch.rachio_zone_6`
- `sensor.rachio_zone_1_*` (next run, last run, duration — verify exact IDs)

---

## Yard Map Tool

A geometrically accurate SVG/HTML yard mapping tool was built in a parallel session. It uses the certified lot survey as its geometric foundation.

**Tool:** https://claude.ai/public/artifacts/5db34a62-c401-4ee7-9e1a-6612241c9d9a

**Features:**
- Two map layers: 💧 Sprinkler Heads (circles, colored by zone) and 🌿 Plants (hexagons, colored by species)
- GPS anchor workflow — stand at the Power Box on the left boundary (~42 yards from NW rear corner), average 40 GPS readings, then 20 readings per item placed; haversine offset math cancels atmospheric error; expect 1–3ft accuracy
- Persistent storage in artifact — survives across sessions
- **Export HA Config button** — generates full `picture-elements` YAML after all heads and plants are placed
- Export JSON (with survey ft positions + raw GPS coords) for reference

**Field session workflow:**
1. Find the Power Box on the left property boundary
2. Average 40 GPS readings at the Power Box to set the anchor
3. Run each Rachio zone, walk to each head, record position (20 GPS readings each)
4. Walk the yard and place each plant species using the plant picker
5. Export JSON as reference backup
6. Export HA Config → copy YAML into dashboard

**Post-field:**
- Export yard map SVG → save to `/config/www/yard_map.svg` on HAOS
- Build `picture-elements` dashboard card using exported YAML
- Overlay: Rachio zone state badges, soil moisture values, WS90 rain rate

---

## Outdoor Plant Inventory

17 species documented with critical care notes. Listed in order of "will cause real damage if ignored":

| Abbr | Species | Critical Rule |
|---|---|---|
| BHY | Bigleaf Hydrangea | Blooms OLD wood — NEVER prune fall/winter/spring. Only right after summer bloom |
| AZA | Azalea (Robin Hill) | Prune within 3 weeks of bloom finishing — miss window = no blooms next year |
| RHO | Rhododendron | Same old-wood rules as Azalea |
| DOG | Flowering Dogwood | Most drought-sensitive plant — water 2–3× per week in summer heat |
| JMG | Japanese Maple (Grafted, front) | Remove ALL green rootstock shoots at base — monthly April–September |
| KOR | Knockout Rose | Japanese beetles June–July; prune 1/3 in late Feb/March |
| AHY | Annabelle Hydrangea | Blooms NEW wood — cut back hard (12–18") in late Feb/March |
| CRM | Crape Myrtle | NEVER top. Remove suckers only |
| JML | Japanese Maple (Laceleaf) | Minimal pruning; preserve natural form |
| RBD | Eastern Redbud Forest Pansy (×2) | Watch for canker (branch dieback) |
| NHS | Nellie Stevens Holly | May need certified arborist |
| MAP | Sugar/Red Maple (×2) | Arborist only for any pruning |
| LIG | Ligustrum/Privet | Trim 3×/season, stop September |
| EUO | Euonymus/Holly Hedge | Watch for scale insects |
| DAY | Daylily | Deadhead daily during bloom; drought tolerant |
| NAN | Nandina | Invasive in VA, toxic to birds — consider replacing |
| SWC | Summersweet (Clethra) | Minimal care |

Full detailed care guide: `docs/yard_care_guide.md` *(to create)*

---

## Ecowitt WS90 Integration

**Status:** Hardware on property. HA integration not yet configured.

**Setup steps:**
1. Install HACS Ecowitt integration (search "Ecowitt")
2. In Ecowitt app: Settings → Upload to → Custom Server → point to HAOS IP on port 4872
3. HA Ecowitt integration auto-discovers entities on first push
4. Verify entity IDs in Developer Tools → States (search "ecowitt")

**Expected entities (verify exact IDs after setup):**
- `sensor.ecowitt_ws90_rain_rate` — real-time rain rate (in/hr)
- `sensor.ecowitt_ws90_rain_daily` — total inches today ← key for watering logic
- `sensor.ecowitt_ws90_temperature` — outdoor temp
- `sensor.ecowitt_ws90_wind_speed` — for wind-hold automation
- `sensor.ecowitt_ws90_uv_index`
- `sensor.ecowitt_ws90_humidity`

**Rachio weather bridge:** In Ecowitt app, enable Weather Underground upload (free). In Rachio app: Settings → Weather Intelligence → select your WU station ID. This gives Rachio actual rainfall data from your yard for Flex Daily scheduling.

---

## Automations to Build

| Automation | Trigger | Action | Priority |
|---|---|---|---|
| Dogwood drought alert | No rain 3+ days AND temp > 85°F | Notify Lindsay: "Deep water Dogwood today" | HIGH |
| Rachio wind hold | `sensor.ecowitt_ws90_wind_speed` > 15 mph | Pause any running Rachio zone | HIGH |
| Rachio rain skip | `sensor.ecowitt_ws90_rain_daily` > 0.25" | Suppress next scheduled zone runs | HIGH |
| Zone soil moisture gating | `sensor.soil_moisture_front` > 60% | Don't run Zone 1; `sensor.soil_moisture_rear` > 60% → don't run Zone 4/5 | MEDIUM |
| FLF moisture alert | Any FLF sensor < 30% | Notify Lindsay: "[Plant name] needs water" | HIGH |
| Japanese beetle season | June 1 entry | Weekly notification Jun 1–Jul 31: "Check Knockout Roses for Japanese beetles" | MEDIUM |
| Rootstock shoot check | Monthly April–September | Notify Lindsay: "Check front Japanese Maple for green rootstock shoots" | MEDIUM |
| Azalea pruning window | ~May 5 (post-bloom) | One-time notify: "Azalea pruning window open — you have ~3 weeks" | HIGH |
| Rhododendron pruning | ~May 5 (post-bloom) | One-time notify: "Rhododendron — deadhead and lightly shape now" | HIGH |
| Bigleaf Hydrangea warning | March 1 | Notify: "Do NOT prune Bigleaf Hydrangea until after summer bloom" | HIGH |
| Knockout Rose pruning | March 1 | Notify: "Prune Knockout Roses by 1/3 now" | HIGH |
| Annabelle Hydrangea pruning | March 1 | Notify: "Cut Annabelle Hydrangea back hard to 12–18 inches now" | HIGH |
| Fertilizer stop | August 31 | Notify both phones: "Stop fertilizing today — last day before hardening season" | MEDIUM |
| FLF last-watered reminder | 7 days without watering (non-sensor plants) | Notify Lindsay | LOW |

---

## Dashboard: Garden Tab

**Location:** New view in Home Command Dashboard, path `garden`

**Section 1 — Indoor Plants**
- 3 FLF moisture tiles: plant name, moisture %, color-coded (green ≥ 50% / yellow 30–50% / red < 30%), days since watered, Mark Watered button, 7-day sparkline
- 4 last-watered tiles: plant name, days since watered, Mark Watered button
- Plant photos as card backgrounds (upload to `/config/www/plants/`)

**Section 2 — Outdoor Care**
- This Month's Tasks: dynamic Markdown card keyed to `now().month` → HIGH priority tasks from Yard Care Guide
- Weather context: outdoor temp, rain today (Ecowitt), UV index, days since last rain
- 6 critical plant status cards (BHY, AZA, RHO, DOG, JMG, KOR) — shows active care window if applicable
- Active alert tiles (appear only when: Japanese beetle season active, pruning window open, drought conditions)

**Section 3 — Irrigation**
- Yard map `picture-elements` card (generated from Yard Map Tool export)
- 6 zone tiles: name, status, next scheduled run, "Run Now" button (with confirmation)
- Ecowitt weather summary: rain today, temp, wind speed

---

## Phases

### Phase 1 — Irrigation Hardware (do first)
- [ ] Purchase Rachio 3 6-zone controller (~$200)
- [ ] Purchase Ecowitt WS90 if not yet on property (confirmed on property)
- [ ] Install Rachio, remove B-Hyve
- [ ] Configure Rachio HA integration
- [ ] Configure Ecowitt HA integration (HACS)
- [ ] Set up Ecowitt → Weather Underground upload
- [ ] Point Rachio Weather Intelligence at WU station ID

### Phase 2 — Yard Mapping Field Session
- [ ] Run field session: GPS anchor at Power Box → map all 6 zone heads → place all plants
- [ ] Fill in zone → key plants table above
- [ ] Export HA Config YAML from Yard Map Tool
- [ ] Export SVG → upload to `/config/www/yard_map.svg`
- [ ] Pair outdoor ThirdReality soil moisture sensors to ZHA
- [ ] Verify all Ecowitt + Rachio + soil sensor entity IDs in HA Developer Tools

### Phase 3 — Dashboard Build
- [ ] Create Garden tab in Home Command Dashboard
- [ ] Indoor plant section (7 tiles)
- [ ] Outdoor care section (monthly task card + weather + 6 plant cards)
- [ ] Irrigation section (picture-elements zone map + 6 zone control tiles)

### Phase 4 — Automations
- [ ] FLF moisture alert → Lindsay's phone
- [ ] Dogwood drought alert
- [ ] Rachio wind hold
- [ ] Rachio rain skip
- [ ] Soil moisture zone gating
- [ ] All seasonal/pruning calendar automations

### Phase 5 — Polish
- [ ] Upload plant photos to `/config/www/plants/`
- [ ] Plant health log (`input_text` per plant)
- [ ] Moisture history sparklines (apexcharts 7-day)
- [ ] Create `docs/yard_care_guide.md` from the docx source

---

## Notes

- Property survey details, lot dimensions, and address: not committed to repo — store in notes or secrets
- Yard Map Tool artifact URL: shared in conversation — not committed (contains property details)
- All seasonal automations send to `notify.mobile_app_lindsays_iphone`
- Fertilizer stop and high-urgency alerts send to both phones
- Nandina replacement decision: Virginia Sweetspire, Inkberry Holly, or Dwarf Fothergilla — decide before next planting season

---

*Last updated: 2026-05-29*
*Status: Phase 1 in progress — Rachio 3 ordered (not yet installed); Ecowitt WS90 planned (not yet ordered)*
