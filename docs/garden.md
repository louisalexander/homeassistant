# Garden & Plants

Lindsay's dedicated dashboard for monitoring indoor plants and managing the outdoor landscape. Integrates soil moisture sensors, a personal weather station, smart irrigation, and a seasonal care calendar into one place.

!!! tip "Yard Map Tool"
    The GPS-anchored yard mapping tool (sprinkler heads + plant positions) lives here: [Yard Map Tool](https://claude.ai/public/artifacts/5db34a62-c401-4ee7-9e1a-6612241c9d9a)

---

## Indoor Plants

Seven indoor plants are tracked on the Garden tab. The three Fiddle Leaf Figs get soil moisture sensors — they're the most sensitive to over- and underwatering. The remaining four use a last-watered timer.

| Plant | Location | Monitoring |
|---|---|---|
| Maury River Fiddle Leaf | Kitchen | Soil moisture sensor (Zigbee) |
| Fiddle Leaf Fig #2 | TBD | Soil moisture sensor (Zigbee) |
| Fiddle Leaf Fig #3 | TBD | Soil moisture sensor (Zigbee) |
| Louis' Monstera | Kitchen | Last-watered timer |
| Bird of Paradise | Kitchen | Last-watered timer |
| Monstera #2 | TBD | Last-watered timer |
| Monstera #3 | TBD | Last-watered timer |

### Moisture Alerts

When a Fiddle Leaf Fig sensor drops below 30%, Lindsay gets a push notification identifying the specific plant. The dashboard tile turns red below 30%, yellow at 30–50%, green at 50%+.

### Last-Watered Tiles

For the non-sensor plants, each tile shows days since last watered and includes a **Mark Watered** button. If 7 days pass without logging a watering, Lindsay gets a reminder.

!!! note "Sensors on order"
    The indoor ThirdReality Gen2 Zigbee sensors are on order. Until they arrive and are paired to ZHA, the FLF tiles will show the last-watered timer layout instead.

---

## Outdoor Plant Care

### Seasonal Care Calendar

The Garden tab's Outdoor section shows a **This Month's Tasks** card — a dynamic list of the most time-sensitive care tasks for the current month, pulled from the 17-species care guide. The goal is to surface the plants where missing a window causes real damage.

| Plant | What Goes Wrong If You Miss It |
|---|---|
| Bigleaf Hydrangea | Blooms on old wood — pruning fall/winter kills next year's blooms entirely |
| Azalea & Rhododendron | 3-week pruning window right after bloom — miss it and no blooms next year |
| Flowering Dogwood | Most drought-sensitive plant on the property — needs 2–3× deep watering per week in summer heat |
| Grafted Japanese Maple (front) | Rootstock green shoots at the base must be removed monthly, April–September — if they take over, the graft dies |
| Knockout Roses | Japanese beetles June–July require active management; prune 1/3 in late Feb/March |
| Annabelle Hydrangea | Blooms on new wood — cut back hard (12–18") in late Feb/March |
| Crape Myrtle | Never top — remove suckers only |

### Automated Reminders

These notifications fire automatically at the right time each season:

| Alert | When | Who Gets It |
|---|---|---|
| Dogwood drought warning | No rain 3+ days AND temp > 85°F | Lindsay |
| FLF moisture alert | Sensor drops below 30% | Lindsay |
| Azalea pruning window open | ~May 5 (post-bloom) | Lindsay |
| Rhododendron deadhead reminder | ~May 5 | Lindsay |
| "Do NOT prune Bigleaf Hydrangea" warning | March 1 | Lindsay |
| Knockout Rose pruning reminder | March 1 | Lindsay |
| Annabelle Hydrangea hard-cut reminder | March 1 | Lindsay |
| Japanese beetle season check | Weekly, June 1 – July 31 | Lindsay |
| Japanese Maple rootstock check | Monthly, April – September | Lindsay |
| Stop fertilizing | August 31 | Both phones |

---

## Weather Station

An **Ecowitt WS90** all-in-one weather station is planned for the property. It measures temperature, humidity, wind speed, UV index, and rainfall using an ultrasonic sensor and haptic rain detector — no moving parts to clog or wear out.

The station pushes readings locally to Home Assistant every 60 seconds over your home network. No cloud account is required.

The Garden tab shows:
- Current outdoor temperature
- Rain today (inches)
- Days since last measurable rain
- UV index
- Wind speed (relevant for irrigation hold)

### Weather-Driven Irrigation

The WS90 is also connected to Weather Underground (WU), which feeds directly into Rachio's **Weather Intelligence+** scheduling. Rachio reads your actual on-property rainfall and calculates daily evapotranspiration (how much water the soil loses from sun and wind). It automatically skips zones after enough rain and adjusts zone run times based on recent weather — rather than running on a fixed schedule regardless of conditions.

!!! note "Integration pending"
    The Ecowitt HA integration has not yet been configured. Setup requires installing the Ecowitt integration via HACS and pointing the Ecowitt app to your HAOS IP.

---

## Irrigation

The property has six irrigation zones controlled by a **Rachio 3** smart controller.

| Zone | Name | Area |
|---|---|---|
| 1 | Front | Front lawn and beds |
| 2 | Left Side | Left side yard |
| 3 | Right Side | Right side yard |
| 4 | Rear Left | Rear left |
| 5 | Rear Right | Rear right |
| 6 | Drip/Garden | Drip lines and garden beds |

### Dashboard Controls

The Irrigation section of the Garden tab shows all six zones with:
- Current status (running / idle / skipped)
- Next scheduled run time
- A **Run Now** button (requires confirmation) for manual watering

### Smart Zone Gating

Two outdoor soil moisture sensors (front yard and rear yard) prevent irrigation from running when the soil is already wet:

- If Front yard sensor > 60% moisture → Zone 1 will not run
- If Rear yard sensor > 60% moisture → Zones 4 and 5 will not run

### Wind Hold

If wind speed exceeds 15 mph, any running Rachio zone is automatically paused to prevent spray drift wasting water on the pavement.

!!! note "Hardware pending"
    Rachio 3 purchase is pending. Current controller is Orbit B-Hyve. Until Rachio is installed, dashboard controls and smart gating are not active. Outdoor soil sensors are deployed in the yard and will pair to ZHA when the full integration is configured.

---

## Yard Map

After the irrigation field session is complete, the Irrigation section will show a **property map** with all six zone coverage areas and sprinkler head positions. The map is generated from a GPS-anchored survey and exported as a Home Assistant picture-elements overlay — zone status badges and soil moisture readings will appear directly on the map image.
