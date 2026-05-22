# Smoke & CO Detectors — Whole-Home Replacement

## Situation

All smoke/CO detectors are original to the 2006 build — approximately 20 years old. NFPA recommends replacing smoke detectors every 10 years. The entire house needs replacement simultaneously (not room by room) to maintain proper interconnection.

**Hard constraints:**
- No new wiring — must use existing infrastructure
- Whole-home project — all units replaced at once to ensure interconnect compatibility
- HA integration desired (alert notifications, Alarmo trigger, dashboard visibility)

---

## Existing Wiring

2006 construction almost certainly has 120V hardwired smoke detectors with the standard 3-wire interconnect:
- **Hot** (black) — line power
- **Neutral** (white)
- **Interconnect** (typically orange or yellow) — when one triggers, all trigger

Any replacement unit using the same 3-wire hardwired standard will work with existing wiring. The interconnect wire handles the "all go off together" requirement natively — no smart home hub needed for that core safety function.

---

## Protocol Options

### Option 1 — First Alert Z-Wave ZCOMBO-G (Recommended)
- 120V hardwired + battery backup
- Smoke + CO combo
- Z-Wave Plus — pairs directly to existing Z-Wave dongle in HA
- HA Z-Wave integration exposes: smoke detected, CO detected, battery level, tamper
- Interconnects via existing hardwire interconnect wire (not Z-Wave)
- Local control, no cloud dependency
- **~$45–55/unit**

**Why this works well here:** Z-Wave dongle is already installed. No new protocol, no new hub, fully local. Z-Wave mesh improves with each unit added.

### Option 2 — Nest Protect (Wired)
- 120V hardwired, wirelessly interconnects via Nest proprietary protocol (no interconnect wire needed, but existing wire still carries power)
- Smoke + CO + steam sense (reduces false alarms in bathrooms)
- HA integration via Google / Nest integration (cloud-dependent)
- Best-in-class UX and false-alarm reduction; path light feature
- **~$119/unit**
- **Con:** Google account dependency, cloud integration, significantly more expensive

### Option 3 — Kidde/First Alert with HomeKit
- Some models support HomeKit natively → flows into HA via HomeKit Device integration
- 120V hardwired
- More limited Z-Wave/Zigbee ecosystem support
- **~$35–50/unit**

---

## Unit Count

**9 units confirmed** (counted 2026-05-21).

- [x] Count all existing detector locations — **9**
- [x] 3-wire interconnect confirmed at all 9 locations

---

## Recommended Approach

**First Alert Z-Wave ZCOMBO-G** — uses existing Z-Wave dongle, no new protocol, local control, hardwired with battery backup, combo smoke + CO. 

Cost at 9 units: **$405–495 total** vs. Nest Protect at **$1,071**.

Nest Protect is the better product but the cost premium and Google cloud dependency are hard to justify when a solid local option exists.

---

## Integration with HA / Alarmo

Once installed:
- Z-Wave entities: `binary_sensor.[room]_smoke` and `binary_sensor.[room]_co`
- Add these sensors to Alarmo as trigger sources alongside door/window sensors
- Automation: smoke detected → Sonos TTS announcement + critical mobile notification (bypass DND) + Alarmo trigger
- Dashboard: binary sensor badges on Security tab

---

## Open Questions

- [x] Count all existing detector locations — **9 confirmed**
- [x] 3-wire interconnect confirmed at all 9 locations
- [ ] Decide: Z-Wave ZCOMBO-G vs. Nest Protect — finalize before ordering
- [ ] Are any locations battery-only (no hardwire)? If so, need battery-powered unit for those locations

*Last updated: 2026-05-21*
