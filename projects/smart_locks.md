# Smart Locks — Planning

## Status
Considering Schlage smart locks. No model chosen. Doors to cover TBD.

---

## Constraints & Goals

- **Protocol:** Prefer Z-Wave (existing dongle installed) or Zigbee (existing ZHA). Avoid WiFi-only (cloud dependency).
- **Brand:** Schlage is the current preference — known for grade 1 deadbolt quality
- **HA integration:** Full local control via Z-Wave or Zigbee. Lock/unlock, battery level, access codes
- **No new wiring:** Battery-powered locks only (standard for smart deadbolts)

---

## Schlage Z-Wave Models to Evaluate

| Model | Protocol | Form | Notes |
|---|---|---|---|
| Schlage BE469ZP | Z-Wave Plus | Touchpad deadbolt | Most common HA choice; keypad + Z-Wave; no built-in alarm |
| Schlage BE489WB | Z-Wave Plus | Touchpad + WiFi | Z-Wave primary, WiFi backup; avoid if WiFi adds cloud dependency |
| Schlage BE468CZP | Z-Wave Plus | No keypad | Key-only exterior; less useful for smart home |
| Schlage Encode Plus | Matter | Touchpad | Matter over WiFi; no Z-Wave; requires Matter hub — skip |

**BE469ZP is the standard recommendation** for HA + Z-Wave setups. Widely tested, grade 1 deadbolt, keypad for manual entry, full Z-Wave integration with HA.

---

## Doors to Decide

- [ ] Front door
- [ ] Rear door
- [ ] Garage personnel door (already in exterior lighting plan)
- [ ] Others?

Not every door necessarily needs a smart lock. Prioritize entry points with the most use or security value.

---

## HA Integration

Z-Wave lock entities expose:
- `lock.front_door` — lock/unlock state + service calls
- Battery level attribute
- Access code slots (set/delete codes via Z-Wave JS UI or HA service)
- Lock/unlock events for automations (e.g. "front door unlocked → turn on foyer lights")

**Code management:** Z-Wave JS UI add-on provides a code management interface. Alternatively, use the `zwave_js.set_lock_usercode` service in automations.

**Alarmo integration:** Lock state can be a condition in Alarmo (e.g. arm away only if all doors locked, or notify if armed while a door is unlocked).

---

## Brainstorm Needed

Before ordering, decide:
- Which doors get locks
- Keypad requirement per door (some exterior doors may be key-only fine)
- Whether a single brand/model across all doors simplifies code management
- Budget per lock (BE469ZP ~$150–180/unit)

*Last updated: 2026-05-21*
*Status: Planning — model and door selection pending*
