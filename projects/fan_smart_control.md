# Fan Smart Control — RF Analysis & Switch Replacement

> **Status: Superseded — 2026-05-21**
> RF diagnostic path evaluated and dropped. Canopy module replacement (Phase 3) adopted as the approach for all fans including the family room Hunter. The family room fan is now a line item in the Inovelli lighting plan (Batch 6). No further work needed in this file.
> See: `projects/inovelli_lighting.md` → Batch 6

---

## Problem Statement

Single-zone ceiling fan with integrated light kit, controlled by a proprietary RF remote. The fan is powered by a single circuit in a 3-way switch configuration. When either switch cuts power, the fan and light do not restore their previous state on power-up — they must be manually re-engaged via the RF remote. This makes smart automation unreliable.

## Hardware Context

- Fan: Hunter (model TBD — check label inside canopy)
- Remote frequency: TBD — Hunter remotes typically 303 MHz or 433.92 MHz
- Remote protocol: TBD — likely fixed-code (confirm with Flipper capture)
- Switch configuration: 3-way
- Current wall control: cuts mains power to the entire fan unit

## Goal

Reliable HA control of fan speed and light independently, with the 3-way configuration preserved, without the state-loss-on-power-cut problem.

---

## Phase 1 — RF Analysis with Flipper Zero

**Steps:**
1. On Flipper: Sub-GHz → Read RAW
2. Point remote 6–12 inches from Flipper, press each button separately:
   - Fan off
   - Fan low / medium / high
   - Light toggle (or on/off separately if separate buttons)
3. Save each capture with a descriptive name (e.g., `fan_light_toggle`, `fan_speed_high`)
4. Note the detected frequency

**Key question: fixed code or rolling code?**
- Fixed code → same signal every press → can replay reliably → proceed to Phase 2
- Rolling code → signal changes each press → much harder to replicate → may need to go straight to canopy module replacement (Phase 3)

**Record findings here:**
- Frequency: ___
- Protocol: ___
- Code type (fixed/rolling): ___
- Remote FCC ID (printed on back of remote): ___

---

## Phase 2 — HA Integration via RF Replay (if fixed code)

Keep the existing fan receiver. Add an RF transmitter that HA can control to send the captured signals.

**Hardware options:**
- **Sonoff RF Bridge** (~$15) flashed with Tasmota — easiest, WiFi, native HA integration
- **CC1101 module + ESP32** running ESPHome — more flexible, handles 303/433 MHz, can be DIY or use pre-built boards like the SlimmeLezer or similar

**Wall switch approach:**
- Wire the wall switches so they **never cut power to the fan** (always-on pass-through)
- All fan/light control goes through HA → RF transmitter → fan receiver
- 3-way switches become decorative unless reprogrammed as HA scene controllers

**HA automation needed:**
- Button card or dashboard control for fan speed + light
- Optional: sync switch presses to RF commands via ESPHome button entity

---

## Phase 3 — Canopy Module Replacement (cleanest long-term solution)

Replace the fan's RF receiver entirely with a smart canopy module. Fan always has mains power; control signals travel over Zigbee/Z-Wave instead of cutting the circuit.

**Recommended hardware:**
- **Inovelli VZW36** Blue Series Fan + Light Wall Switch (~$60) — Zigbee
- **Inovelli VZM35-SN** Fan + Light Canopy Module (~$50) — goes inside fan housing
- **Inovelli Aux Switch** — second location in the 3-way run (~$20)

**How it works:**
- VZM35-SN is wired to hot/neutral in the canopy — always powered
- VZW36 communicates with canopy module over Zigbee
- Aux switch wires to VZW36 via traveler wire — 3-way works natively
- Fan speed and light are independently dimmable from wall + HA
- No RF remote needed; existing remote can be retired

**Wiring requirement:** Neutral wire must be available at the wall switch box. Confirm before ordering.

**HA integration:** VZW36 pairs directly to ZHA. Exposes separate fan speed entity and light entity.

---

## Decision Tree

```
Flipper capture → Fixed code?
  ├─ YES → Try Sonoff RF Bridge / ESPHome CC1101 (Phase 2)
  │         Works reliably? → Done
  │         Flaky/inconsistent? → Go to Phase 3
  └─ NO  → Skip to Phase 3 (canopy module replacement)
```

---

## Open Questions

- [ ] Identify fan make/model and remote FCC ID
- [ ] Complete Flipper capture — confirm frequency and code type
- [ ] Check for neutral wire at both switch boxes
- [ ] Decide: RF replay (Phase 2) vs canopy replacement (Phase 3)

---

## Related

- House energy strategy: `projects/house_energy_strategy.md`
- Relevant HA entities (once implemented): TBD
