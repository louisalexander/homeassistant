# Security & Alarm

## System Architecture (Target State)

```
Konnected Alarm Panel Pro (wired zones)
  └─► Alarmo (HA alarm panel)
        └─► Ring Alarm Gen 2 Base Station (via ring-mqtt)
              └─► Ring Protect Pro (professional monitoring)
```

---

## Alarm System — Konnected

### Status: Hardware in hand — not yet installed

| Component | Status | Notes |
|---|---|---|
| Konnected Alarm Panel Pro | Purchased | 4 zones available |
| GE Concord Express 60-806 | Existing | Being replaced — 4 wired zones |
| Alarmo (HA add-on) | Not yet configured | HA-native alarm panel |
| Ring Alarm Gen 2 Base Station | Not yet ordered | Order after Konnected is commissioned |
| ring-mqtt (HACS) | Not yet installed | Bridges Alarmo → Ring Pro monitoring |

### Existing Wired Zones (GE Concord 60-806)

4 zones are wired and will be reused by Konnected:

| Zone | Location | Sensor type |
|---|---|---|
| Zone 1 | TBD | Door/window contact |
| Zone 2 | TBD | Door/window contact |
| Zone 3 | TBD | Motion |
| Zone 4 | TBD | TBD |

### Mock Door Sensor Automations

Three `input_button` helpers simulate door sensors until Konnected is wired. These are Section 12 automations in `automations.yaml`:

| Helper | Simulates | Action |
|---|---|---|
| `input_button.front_door_test` | Front door open | Ring all 4 chimes — motion tone |
| `input_button.garage_door_test` | Garage door open | Ring all 4 chimes — motion tone |
| `input_button.rear_door_test` | Rear door open | Ring all 4 chimes — motion tone |

When Konnected is wired, replace `input_button.*` trigger entities with `binary_sensor.*` Konnected entities — everything else stays the same.

### Install Sequence

1. Order Ring Alarm Gen 2 Base Station (~$60–100)
2. Install ring-mqtt HACS add-on in HA
3. Wire Konnected to 4 zones + siren
4. Configure Alarmo using Konnected `binary_sensor` entities
5. Pair Ring base station → ring-mqtt → confirm panic switches appear in HA
6. Automation: Alarmo triggered → `switch.ring_alarm_police_panic`
7. Automation: Smoke detector triggered → `switch.ring_alarm_fire_panic`
8. Set up Sonos TTS announcements for alarm events

---

## Leak Detection

7 active leak sensors (ZHA — Third Reality):

| Location | Entity |
|---|---|
| Kitchen sink | `binary_sensor.kitchen_sink_leak_detector` |
| Laundry room | `binary_sensor.laundry_room_leak_sensor` |
| Master bathroom left sink | `binary_sensor.master_bathroom_left_sink_leak_detector` |
| Master bathroom right sink | `binary_sensor.master_bathroom_right_sink_leak_detector` |
| Powder room | `binary_sensor.powder_room_leak_sensor` |
| Office sink | `binary_sensor.office_sink_leak_detector` |
| Bedroom 3 sink | `binary_sensor.bedroom_3_sink_leak_detector` |

!!! danger "Critical alerts"
    Leak notifications bypass DND on both iPhones. No cooldown — every detection alerts.

!!! tip "Adding more sensors"
    2 additional leak sensors are on hand. Pair to ZHA — existing Section 1 automations detect all leak sensor entities automatically. No automation changes needed.

---

## Smoke Detectors

### Status: Not yet ordered

- **9 locations confirmed** (all 3-wire interconnect)
- **Recommended:** First Alert Z-Wave ZCOMBO-G (~$45–55 each, ~$405–495 total)
- **Policy:** Order when ready to install same week — don't leave them sitting uninstalled
- **Install before Konnected** — smoke detectors trigger the Ring fire panic switch via Alarmo

---

## Smart Locks

### Status: Not yet ordered — waiting for hardware backlog to clear

- **Front door first:** Schlage BE469ZP (Z-Wave, ~$160)
- **Integration:** Z-Wave JS → HA → access codes via Z-Wave JS UI
- **Alarmo integration:** Arm away if locked; notify if armed while unlocked
- **Expansion:** Decide additional doors after front door workflow is validated

---

## Garage Doors

Controlled via ESPHome (custom firmware on door openers):

| Door | Entity | Note |
|---|---|---|
| Left (physically right) | `cover.left_garage_door_opener_garage_door` | Naming inverted from installer |
| Right (physically left) | `cover.right_garage_door_opener_garage_door` | See note |

Garage automations (Section 2 + 8):
- Notifications on open/close
- Alert if either door is open when both people are away
- Alert + actionable button if open for 30+ minutes

---

## Presence Detection

Uses `zone.home` person count — not individual person entities.

| Entity | Description |
|---|---|
| `zone.home` | Number of people in the Home zone |
| `person.louis` | Louis's location |
| `person.lindsay` | Lindsay's location |

"Away" = `zone.home == 0` (everyone left). "Home" = `zone.home >= 1` (anyone arrived).
