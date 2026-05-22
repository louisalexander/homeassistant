# Climate System — Architecture Reference

## Overview

Two Ecobee thermostats control the house in `heat_cool` mode (dual setpoint). Both are integrated into HA via the official Ecobee integration. Ecobee Eco+ TOU (Time of Use) is enabled in the Ecobee app — it learns the home's thermal characteristics and pre-conditions before peak pricing periods. HA does not run any thermal battery automations; Ecobee handles all pre-conditioning adaptively.

---

## Hardware & Entities

### HVAC System
- **Type:** Dual-fuel / hybrid heat pump + gas auxiliary
- **Air handler:** Trane TEM4A0B24S21SBA
- **O/B terminal:** Set to "O" (reversing valve energizes on cooling — correct for most heat pumps)
- **Configuration:** Auto (heat_cool) mode; minimum heat/cool delta = 5°F (Ecobee default)
- **Floors:** 3 HVAC zones; Ecobee on 2nd floor (master bed) and 3rd floor (office) are active; **1st floor Ecobee purchased but not yet installed**

### 1st Floor HVAC — Crawlspace Unit
- **Furnace:** York PS9B12N080DH11B — gas, 80,000 BTU, 80% AFUE, 2005 vintage, located in crawlspace
- **Current thermostat:** Honeywell (multi-stage, conventional mode — Y2, W2, G, W, Y, R, Rc terminals used)
- **Thermostat cable:** Multi-conductor cable run from 1st floor thermostat location down through floor to crawlspace furnace. Cable may be entirely unconnected at the furnace end (yellow wire visible dangling at furnace in photos — not yet terminated on control board).
- **C wire status:** Blue conductor exists in cable but is NOT connected at either end. This is the primary installation challenge — must connect blue wire to C terminal on York control board during crawlspace visit.

**Ecobee wiring map (conventional gas + AC, no heat pump on this zone):**

| Wire color | Thermostat terminal | York control board terminal |
|---|---|---|
| Red | Rc | R |
| Blue | C | C or COM |
| Green | G | G |
| White | W | W (gas valve) |
| Yellow | Y | Y (compressor/cool) |

**Installation steps:**
1. Go into crawlspace with phone (for photos), flathead/Phillips screwdriver, labels
2. Open York lower access panel → find low-voltage terminal block
3. Photo the terminal block before touching anything
4. If cable is entirely unconnected: land all wires per table above
5. If partially connected: find the loose blue wire → land on C terminal
6. At thermostat: swap Honeywell for Ecobee base, land wires per Ecobee terminal labels
7. Ecobee setup: select "Conventional" system type (not heat pump) for this zone

### Thermostats

| Friendly Name | Entity | Covers | Status |
|---|---|---|---|
| Master Bedroom | `climate.bedroom` | Master bedroom zone | Active |
| Louis' Office | `climate.office` | Office zone | Active |
| 1st Floor | TBD | 1st floor zone | **Not yet installed** |

### Temperature Sensors

| Sensor | Location | Normal Setpoint |
|---|---|---|
| `sensor.bedroom_temperature` | Master bedroom | 75°F cool / 69°F heat |
| `sensor.bedroom_1_temperature` | Bedroom 1 | 75°F cool |
| `sensor.bedroom_2_temperature` | Bedroom 2 | 75°F cool |
| `sensor.bedroom_3_temperature` | Bedroom 3 | 75°F cool |
| `sensor.bedroom_4_temperature` | Bedroom 4 | 75°F cool |
| `sensor.office_temperature` | Office | 73°F cool / 68°F heat |

### Key Ecobee Integration Attributes (on climate entities)

| Attribute | Description |
|---|---|
| `preset_mode` | Current comfort setting: `home`, `away`, `sleep`, `away_indefinitely` |
| `climate_mode` | Human label of comfort setting (e.g. `"Home"`) |
| `target_temp_high` | Cooling setpoint in heat_cool mode |
| `target_temp_low` | Heating setpoint in heat_cool mode |
| `current_temperature` | Averaged temperature from active sensors |
| `hvac_action` | Current action: `cooling`, `heating`, `idle`, `fan` |
| `equipment_running` | Comma-separated equipment string e.g. `"compCool1,fan"` |
| `available_sensors` | All paired remote sensors |
| `active_sensors` | Sensors Ecobee is currently averaging for control |

**Important:** The HA integration does not expose hold status. There is no `is_hold`, `hold_type`, or event type attribute. The `preset_mode` stays `"home"` whether following a schedule or in a hold. See Hold Detection below.

---

## Ecobee Eco+ TOU

Eco+ TOU is enabled in the Ecobee app (not configurable from HA). It uses the utility's TOU rate schedule and the home's learned thermal mass to pre-condition before peak periods.

**How it manifests in HA attributes (summer):**

| Phase | `target_temp_high` | Description |
|---|---|---|
| Pre-cool | ≤ 72°F | Actively chilling below normal before peak |
| Normal | 73–75°F | Following comfort program setpoints |
| Peak coast | ≥ 76°F | HVAC off or minimal; coasting on stored cool |
| Weekend / no TOU | Normal setpoints | No Eco+ adjustment on non-peak days |

**Winter equivalent:**

| Phase | `target_temp_low` | Description |
|---|---|---|
| Pre-heat | ≥ 71°F | Heating above normal before peak |
| Normal | 68–70°F | Following comfort program setpoints |
| Peak coast | ≤ 67°F | Coasting on stored heat |

---

## Dashboard — Climate Tab (views[2])

### Card Structure

| Index | Type | Description |
|---|---|---|
| 0 | `horizontal-stack` | Two mushroom-climate-cards — thermostat quick controls |
| 1 | `horizontal-stack` | **Mode indicator cards** — Home/Away/Sleep/Vacation/Hold |
| 2+ | Various | Temperature title, temp tiles, chart, air quality, occupancy |

### Card 0 — Thermostat Controls

Two `custom:mushroom-climate-card` cards side by side for `climate.bedroom` and `climate.office`. Standard mushroom card — shows current temp, setpoints, HVAC action. Tapping opens a detail popup.

### Card 1 — Mode Indicator Row

Two `custom:button-card` cards in a horizontal stack, one per thermostat. Each shows the current Ecobee mode with a color-coded background. This is the primary TOU status indicator.

**Modes and colors:**

| State | Color | Condition |
|---|---|---|
| Home | Green (`#1b5e20→#2e7d32`) | `preset_mode == home` and setpoints at normal |
| Hold | Orange (`#e65100→#f57c00`) | Setpoint deviation >1.5°F from normal (Eco+ TOU or manual hold) |
| Away | Grey (`#37474f→#546e7a`) | `preset_mode == away` |
| Vacation | Grey | `preset_mode == away_indefinitely` |
| Sleep | Dark Blue (`#1a237e→#283593`) | `preset_mode == sleep` |

**Hold detection logic (JS in button-card template):**

```javascript
// Summer (months 5–9): hold if cooling setpoint deviates >1.5°F from normal
var hold = isSummer
  ? Math.abs(cool - NORMAL_COOL) > 1.5
  : Math.abs(heat - NORMAL_HEAT) > 1.5;
```

Normal setpoints used for deviation check:
- `climate.bedroom`: cool = 75°F, heat = 69°F
- `climate.office`: cool = 73°F, heat = 68°F

**Label:** Shows current temperature + active setpoint for the current season.

```javascript
// Summer: shows cool setpoint; Winter: shows heat setpoint
if (isSummer) return curr + '°F · setpt ' + cool + '°F';
return curr + '°F · setpt ' + heat + '°F';
```

### Temperature Tile Rows

Six `custom:button-card` tiles, one per temperature sensor (master bed, bed 1–4, office). Color reflects how far each room deviates from the **live Ecobee setpoint** (not hardcoded absolute bands). The setpoint is read from `states['climate.XXX'].attributes.target_temp_high/low` so tiles automatically adapt when Eco+ TOU shifts the setpoints.

**Sensor → climate entity mapping:**
- All bedroom sensors (`sensor.bedroom_temperature`, `sensor.bedroom_1–4_temperature`) → `climate.bedroom`
- `sensor.office_temperature` → `climate.office`

**Color thresholds (deviation from live setpoint, both directions):**

| Deviation | Color | Meaning |
|---|---|---|
| ≤ 1.5°F | Green | Within spec |
| 1.5–3°F | Orange | Slightly off target |
| > 3°F | Red | Significantly off target |

**Label format:** `75.6°F (+0.6°F)` — current temp plus signed deviation from setpoint. Positive = warmer than setpoint, negative = cooler.

**Season logic:** Summer (months 5–9) compares against `target_temp_high` (cooling setpoint). Winter compares against `target_temp_low` (heating setpoint).

### 24-Hour Temperature Chart

`custom:apexcharts-card` showing all temperature sensors over 24h. Echo/Amazon devices are excluded.

---

## VOC Observations

Ecobee SmartSensors report VOC levels. Evening spike pattern observed:
- Master bedroom: ~1,200–1,300 µg/m³ peak (~8–9 PM)
- Office: ~850 µg/m³ peak (~8 PM)

**Suspected cause:** Cooking activity on the 1st floor distributed throughout the house by the HVAC system. This hypothesis cannot be fully confirmed until the 1st floor Ecobee sensor is installed — it would show whether VOC levels are highest at the source (kitchen) before rising upstairs.

---

## Automations

Thermal battery automations were removed in May 2026 after an architecture review found them inferior to Ecobee Eco+ TOU's adaptive learning. The remaining automations do not touch HVAC setpoints.

**What remains (26 automations in `automations.yaml`):**
- Presence detection (home/away transitions)
- Lighting automations
- Security/camera automations
- Notification automations
- Other non-climate automations

**What was removed:**
- 9 thermal battery automations (pre-cool, pre-heat, peak coast scheduling, season detection, override management)

---

## Hold Detection Limitations

The standard HA Ecobee integration does not expose hold status. The setpoint deviation method used in the mode card is the best available proxy because:

1. Eco+ TOU *always* changes `target_temp_high/low` when active — it cannot adjust setpoints without this being visible
2. Manual temperature holds also change setpoints by definition
3. The only undetectable case: a manual hold set to exactly the normal setpoint (degenerate case, no practical impact)

**If true hold detection is needed in future:** Ecobee Suite Manager (HACS) exposes event type (`hold`, `program`, `demandResponse`, `vacation`) directly. See `projects/ecobee_suite_notes.md` if that investigation is started.

---

## Related Files

- `automations.yaml` — all automations; pushed to HAOS at `/config/automations.yaml`
- Dashboard lives in HAOS at `/config/.storage/lovelace.home_command_dashboard` (JSON)
- `/tmp/add_mode_card.py` — script that inserted the mode indicator row
- `/tmp/fix_status_card_ecobee.py` — script that built the Ecobee-aware status card
- `/tmp/fix_temp_deviation.py` — script that set deviation-from-setpoint coloring on temp tiles (replaced fix_temp_colors.py)
- `projects/house_energy_strategy.md` — energy monitoring project (EM16P)

---

## Dashboard Edit Workflow

```bash
# Pull fresh copy from HAOS before any edit
ssh root@192.168.4.93 "cat /config/.storage/lovelace.home_command_dashboard" > /tmp/lovelace_dashboard.json

# Edit via Python script, then push
scp /tmp/lovelace_dashboard.json root@192.168.4.93:/config/.storage/lovelace.home_command_dashboard

# Restart to load (JSON is read at startup; in-memory state overwrites if HA is running)
ssh root@192.168.4.93 "ha core restart"
```

**Race condition note:** If HA is running when you SCP, HA may overwrite the file on shutdown. Always run `ha core restart` immediately after SCP to minimize the window. Pull fresh before every edit session.
