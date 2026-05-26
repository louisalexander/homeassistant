# Automations

All automations live exclusively in `automations.yaml`. There are currently 27 automations across 14 sections.

!!! info "Editing automations"
    Edit `automations.yaml` locally, SCP to `/config/automations.yaml`, then go to **Developer Tools → YAML → Reload Automations**. No HA restart needed.

---

## Section 1 — Leak Detection

Critical push notifications (bypass DND) sent to both phones immediately on any moisture detection. Notification names the exact room.

| Automation | Trigger | Action |
|---|---|---|
| Leak detected | Any leak sensor → `on` | Critical push notification to both phones naming the room; persistent HA notification |

**7 sensors monitored:** Kitchen sink, laundry room, master bathroom (×2), powder room, office sink, bedroom 3 sink.

!!! danger "Critical alert"
    Leak notifications use `push_type: critical` — they override silent mode and Do Not Disturb on both iPhones.

---

## Section 2 — Garage Doors

| Automation | Trigger | Action |
|---|---|---|
| Garage door opened | Either door → `open` | Push notification to both phones with door name |
| Garage door closed | Either door → `closed` | Push notification to both phones |
| Garage open when away | Door open + both people away | Push notification with actionable "Close It" button |

---

## Section 3 — Salt Lamp

Schedule-based on/off for the salt lamp plug.

---

## Section 4 — Air Quality (Bedroom)

| Threshold | Sensor | Action |
|---|---|---|
| CO₂ > threshold | `sensor.bedroom_carbon_dioxide` | Push notification — bedroom CO₂ alert |
| VOC > threshold | `sensor.bedroom_volatile_organic_compounds` | Push notification — bedroom VOC alert |

---

## Section 5 — Air Quality (Office)

Same pattern as Section 4, using office air quality sensors.

---

## Section 6 — Battery Alerts

Monitors all ZHA devices. Sends a push notification when any device battery drops below threshold. Covers all ~15 Zigbee devices including leak sensors, remote sensors, etc.

---

## Section 7 — Thermostat Presence

Uses `zone.home` person count (not individual person entities) to transition Ecobee presets.

| Automation | Trigger | Action |
|---|---|---|
| Everyone leaves | `zone.home` count → 0 | Set both Ecobee thermostats to `away` preset |
| Someone arrives home | `zone.home` count → ≥1 | Set both Ecobee thermostats to `home` preset |

!!! note "Why zone.home?"
    Using the zone count rather than `person.louis` or `person.lindsay` individually means "home" means *anyone* is home, and "away" means *everyone* has left.

---

## Section 8 — Garage Open Too Long

| Automation | Trigger | Action |
|---|---|---|
| Garage open 30 min | Either door open for 30+ minutes | Push notification to both phones with actionable "Close It" button |

---

## Section 9 — TOU Peak Mode

| Automation | Trigger | Action |
|---|---|---|
| Peak starts | `sensor.tou_period` → `peak` | `input_boolean.peak_mode` → `on` |
| Peak ends | `sensor.tou_period` → anything else | `input_boolean.peak_mode` → `off` |

---

## Section 12 — Mock Door Sensors

Konnected Alarm Panel Pro placeholder automations. Each triggered by an `input_button` helper. Replace trigger entities with real `binary_sensor.*` Konnected entities when the panel is wired.

| Automation | Trigger entity | Cooldown |
|---|---|---|
| Mock — Front Door Open | `input_button.front_door_test` | 2 minutes |
| Mock — Garage Door Open | `input_button.garage_door_test` | 2 minutes |
| Mock — Rear Door Open | `input_button.rear_door_test` | 2 minutes |

**Action:** Ring all 4 chimes (`siren.hallway_siren`, `master_bedroom_siren`, `office_siren`, `garage_siren`) with `tone: motion`.

---

## Section 13 — Ring Camera Snapshot Refresh

Prevents stale thumbnails on Ring camera dashboard tiles during quiet periods (no motion detected).

| Automation | Trigger | Action |
|---|---|---|
| Ring snapshot refresh | Every 2 minutes (`time_pattern`) | `homeassistant.update_entity` on both Ring camera entities |

---

## Cooldown Pattern

The 2-minute mock door cooldowns use this reusable template — no extra helpers required, each automation tracks its own last-triggered timestamp:

```yaml
conditions:
  - condition: template
    value_template: >
      {{ this.attributes.last_triggered is none or
         (now() - this.attributes.last_triggered).total_seconds() > 120 }}
```

Change the seconds value to adjust the cooldown.

---

## Notification Deep Links

Push notifications include a `url:` that deep-links to the relevant dashboard tab. These are stored as template sensors (`sensor.nabu_casa_security_url`, `sensor.nabu_casa_climate_url`) in `configuration_tou_addition.yaml` — values come from `secrets.yaml`. This keeps `!secret` directives out of `automations.yaml`, which fixes the HA automation editor's YAML loader limitation.
