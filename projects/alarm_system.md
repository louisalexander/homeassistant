# Alarm System — Konnected Integration

## Current State

### Existing Panel — GE Concord Express 60-806
Legacy wired alarm panel, active but not integrated with HA.

| Zone | Sensor |
|---|---|
| Zone 1 | Front door |
| Zone 2 | Garage door |
| Zone 3 | Rear door |
| Zone 4 | PIR motion sensor |

- Wired siren: confirmed live (accidentally triggered during inspection)
- Panel will be left in place; siren remains wired to it as a fallback after Konnected install

### Konnected Alarm Panel Pro
- **Status:** Purchased, board on hand, not yet installed
- **Plan:** Replaces GE motherboard, reuses all existing sensor wiring → integrates into HA natively
- All 4 zones + siren carry over to Konnected with no re-wiring of sensors
- **Mock automations live (Section 12 in automations.yaml):** 3 input_button helpers + 3 automations for testing; will replace button triggers with Konnected binary_sensor entities when hardware is wired

---

## Target Architecture

```
Existing wired sensors (4 zones)
    ↓
Konnected Alarm Panel Pro → HA binary sensor entities
    ↓
Alarmo (alarm state machine)
    ↓ "triggered"
HA automation
    ↓
ring-mqtt add-on → Ring Alarm Gen 2 Base Station
    ↓
Ring Protect Pro monitoring center (already paid)
    ↓
Police / Fire / Medical dispatch
    +
Sonos TTS announcements + critical mobile push
```

**Konnected** — Replaces GE panel motherboard; reuses all existing wired sensors. Exposes each zone as a `binary_sensor` in HA.

**Alarmo** — HACS add-on providing full alarm state machine (armed_home, armed_away, disarmed, triggered) driven by Konnected sensor entities.

**Ring Alarm Gen 2 Base Station** (~$60–100, to order) — Bridges HA to Ring's professional monitoring. ring-mqtt exposes police and fire panic buttons as HA switch entities. When Alarmo triggers, an HA automation flips the panic switch → Ring's monitoring center dispatches. Ring Protect Pro (already paid) covers the dispatch cost.

**ring-mqtt** — Community HACS add-on (tsightler). Integrates Ring Alarm base station into HA via MQTT. Exposes: `alarm_control_panel.ring_alarm`, `switch.ring_alarm_police_panic`, `switch.ring_alarm_fire_panic`.

**Sonos TTS** — Zone-specific voice announcements via `media_player.master_bedroom_sonos_one` and `media_player.sonos_play_5`. Example: "Intruder alert — front door" on trigger.

**Key automations needed:**
```yaml
# Intruder alarm → police dispatch
trigger: alarm_control_panel.alarmo → triggered
action: switch.turn_on → switch.ring_alarm_police_panic

# Smoke/CO alarm → fire dispatch  
trigger: binary_sensor.[room]_smoke → on
action: switch.turn_on → switch.ring_alarm_fire_panic
```

---

## Economics

**Ring Protect Plus** ($10/month) is worth subscribing to for camera video history — but its professional monitoring is tied to Ring Alarm hardware only. It does NOT trigger dispatch via Alarmo or Konnected.

**Noonlight for Alarmo/Konnected** is a separate standalone service ($9.99/month). Konnected maintains an official HA integration ([konnected-io/noonlight-hass](https://github.com/konnected-io/noonlight-hass)) with a community V2 API fork for modern HA. When Alarmo triggers, an HA automation turns on the Noonlight switch entity → Noonlight dispatches first responders.

**Konnected board:** No ongoing subscription fee. One-time hardware cost only.

**Decision: Ring Alarm Gen 2 Base Station (~$60–100 one-time)**

| Item | Cost |
|---|---|
| Ring Protect Pro (already purchased) | $200/year (paid) |
| Ring Alarm Gen 2 Base Station | ~$60–100 one-time |
| ring-mqtt add-on | Free |
| Standalone Noonlight | **Not needed** |

Ring Protect Pro's professional monitoring is activated by the base station. Alarmo triggers the base station via ring-mqtt panic switches. No monthly subscription beyond Ring Pro already paid. Pays for itself vs Noonlight in 6–10 months.

---

## Installation Steps (when ready)

1. Cut power to GE panel at breaker
2. Document all zone wiring terminals before removing GE board
3. Mount Konnected board — same enclosure or nearby
4. Wire zones to Konnected terminals (1:1 with existing)
5. Wire siren output from Konnected (can drive existing siren or leave GE panel siren as fallback)
6. Power Konnected via 12V supply or USB
7. Connect to WiFi → install Konnected integration in HA
8. Install Alarmo via HACS; configure zones and modes
9. Configure Noonlight integration if using professional dispatch
10. Build automations: Sonos TTS on trigger, mobile critical notifications

---

## Open Questions

- [ ] Confirm doorbell transformer location (searched crawlspace/pantry/near panel; TBD in 2006 home)
- [ ] Decide Ring Alarm vs. Konnected strategy — run both or consolidate?
- [ ] Confirm Noonlight pricing and whether it fits the use case
- [ ] Are there additional wired zones beyond the 4 confirmed? Check panel wiring on install day.

---

## Related
- `projects/smoke_detectors.md` — smoke sensor integration will feed into Alarmo once installed
- Ring cameras active in HA for visual verification on alarm events

*Last updated: 2026-05-21*
