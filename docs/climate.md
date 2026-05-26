# Climate System

## Overview

Two Ecobee thermostats control the house in `heat_cool` mode (dual setpoint). **Ecobee Eco+ TOU is the thermal management system** — HA does not write HVAC setpoints. All pre-conditioning is handled adaptively by Ecobee's learning algorithm.

---

## Thermostats

| Thermostat | Entity | Zone | Status |
|---|---|---|---|
| Master Bedroom | `climate.bedroom` | 2nd floor master bed zone | Active |
| Louis' Office | `climate.office` | 3rd floor office zone | Active |
| 1st Floor | TBD | 1st floor zone | **Hardware in hand — not yet installed** |

### 1st Floor Install

The 1st floor Ecobee needs one crawlspace trip:

1. Locate York PS9B12N080DH11B furnace low-voltage terminal block
2. Land blue wire on **C terminal** (currently unconnected at both ends)
3. Wire per table: Red→Rc, Blue→C, Green→G, White→W, Yellow→Y
4. Swap Honeywell thermostat for Ecobee base; configure as **Conventional** system (not heat pump)

---

## HVAC Hardware

- **Air handler:** Trane TEM4A0B24S21SBA
- **System type:** Dual-fuel / hybrid heat pump + gas auxiliary
- **O/B terminal:** Set to "O" (reversing valve energizes on cooling — correct)
- **Configuration:** Auto (`heat_cool` mode), min heat/cool delta = 5°F

---

## Ecobee Eco+ TOU

Eco+ TOU is enabled in the Ecobee app (not configurable from HA). It uses the utility's TOU rate schedule and the home's learned thermal mass to pre-condition before peak pricing periods.

### How it appears in HA attributes

**Summer (months 5–9):**

| Phase | `target_temp_high` | Description |
|---|---|---|
| Pre-cool | ≤ 72°F | Chilling below normal before peak window |
| Normal | 73–75°F | Following comfort program |
| Peak coast | ≥ 76°F | HVAC minimal; coasting on stored cool |

**Winter:**

| Phase | `target_temp_low` | Description |
|---|---|---|
| Pre-heat | ≥ 71°F | Heating above normal before peak window |
| Normal | 68–70°F | Following comfort program |
| Peak coast | ≤ 67°F | Coasting on stored heat |

---

## Dashboard — Climate Tab

### Card 1 — Thermostat Controls
Two `mushroom-climate-card` cards side by side. Tap to open setpoint controls.

### Card 2 — Mode Indicator Row
Two `button-card` per thermostat showing current Ecobee mode with color-coded background.

| State | Color | Condition |
|---|---|---|
| Home | Green | `preset_mode == home`, setpoints at normal |
| Hold | Orange | Setpoint deviation >1.5°F from normal (Eco+ TOU or manual hold) |
| Away | Grey | `preset_mode == away` |
| Vacation | Grey | `preset_mode == away_indefinitely` |
| Sleep | Dark blue | `preset_mode == sleep` |

**Hold detection:** The HA Ecobee integration doesn't expose hold status. Mode cards infer it from setpoint deviation:
```javascript
// Summer: hold if cooling setpoint deviates >1.5°F from normal (75°F bedroom, 73°F office)
var hold = isSummer
  ? Math.abs(cool - NORMAL_COOL) > 1.5
  : Math.abs(heat - NORMAL_HEAT) > 1.5;
```

### Card 3 — Temperature Tiles

Six `button-card` tiles (one per sensor) colored by deviation from the **live Ecobee setpoint** — automatically adapts when Eco+ shifts setpoints.

| Deviation | Color | Summer meaning |
|---|---|---|
| ≤ −3°F | Deep blue | Great — very cool |
| −3 to −1.5°F | Light blue | Good — below setpoint |
| −1.5 to +1.5°F | Green | On target |
| +1.5 to +3°F | Orange | Slightly warm |
| > +3°F | Red | Too warm |

Labels show: `75.6°F (+0.6°F)` — current temp plus signed deviation.

---

## TOU Rate Sensors

Defined in `configuration_tou_addition.yaml`, re-evaluated every minute:

| Entity | Description |
|---|---|
| `sensor.tou_period` | `peak` / `off_peak` / `super_off_peak` |
| `sensor.tou_rate` | Current rate in $/kWh |
| `input_boolean.peak_mode` | `true` during TOU peak hours |

**Schedule (Dominion Energy Schedule 1G):**

| Period | Summer (May–Sep) weekdays | Winter weekdays |
|---|---|---|
| Peak | 3–6 PM | 6–9 AM and 5–8 PM |
| Super off-peak | Midnight–5 AM | Midnight–5 AM |
| Off-peak | All other times | All other times |

Weekends: no peak; only off-peak and super off-peak (midnight–5 AM).

---

## VOC Observations

Evening VOC spike pattern (suspected cooking from 1st floor distributing via HVAC):
- Master bedroom: ~1,200–1,300 µg/m³ peak around 8–9 PM
- Office: ~850 µg/m³ peak around 8 PM

Hypothesis unconfirmed until 1st floor Ecobee sensor is installed — it would show whether kitchen VOC levels peak before rising upstairs.
