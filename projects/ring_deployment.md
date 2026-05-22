# Ring Ecosystem Deployment

## Hardware Inventory

| Device | Qty | Status |
|---|---|---|
| Ring Indoor Cam (plug-in, like garage unit) | 1 | Not placed |
| Ring Outdoor Cam + Solar Panel (w/ pole + wall mount) | 2 | Not mounted |
| Ring Video Doorbell Powered 2K | 1 | Not installed |
| Ring Doorbell Transformer (upgrade) | 1 | In hand — not yet installed |
| Ring Chime V3 | 4 | Not placed |
| Ring Chime (original / v1) | 1 | **Removed from Ring account 2026-05-21** — retired |
| Ring Chime Pro (original) | 1 | **Removed from Ring account 2026-05-21** — retired |

> **Chime status:** Ring Chime v1 and Chime Pro are registered in account but unplugged. **Deprecate both** — remove from Ring app, replace with 4× V3 chimes. V3 covers all 4 placement zones; no need to keep legacy units.

---

## Doorbell Upgrade — Ring Powered 2K

The Powered 2K requires a hardwired connection with sufficient transformer capacity. Most pre-2010 homes have a 10–16V 10VA transformer — the Powered 2K needs **16–24V AC, 30VA minimum**.

**Steps:**
1. Locate existing doorbell transformer (typically near electrical panel, in a utility closet, or in the SMC)
2. Replace with the Ring-supplied transformer (or a compatible 16V 30VA unit)
3. Run existing doorbell wiring from transformer to doorbell location
4. Install Ring Pro Power Kit (included) at the indoor chime — prevents voltage drop under load
5. Mount Powered 2K at front door, connect to wired terminals
6. Register in Ring app → auto-appears in HA Ring integration

**Wiring gotcha:** If the existing doorbell is battery-only Ring (no wired terminals used), you'll need to run a wire from the transformer to the doorbell location. Measure the run before committing — standard 18 AWG doorbell wire works up to ~100 ft.

---

## Outdoor Cameras — Mounting

**Proposed location:** Gutter downspout mounts, pointing at driveway/yard coverage zones.

**Hardware needed:** Downspout brackets — **in hand**.

**Pre-mount checklist per camera:**
- [ ] Confirm the downspout faces the coverage zone (not toward a neighbor or wall)
- [ ] Solar panel has 4+ hours direct sun at that location (check for shade from roofline or trees)
- [ ] Camera is within Ring's Wi-Fi range (or Chime Pro can extend it — see chime placement below)
- [ ] Camera elevation gives useful view angle (8–10 ft is typical for good plate/face coverage)

**Placement decisions needed:**
- [ ] Camera 1 location — driveway? side yard? rear?
- [ ] Camera 2 location — ?

---

## Indoor Camera — Placement

One plug-in indoor cam (same form factor as the garage unit). Likely candidates:
- [ ] Mudroom / entry area (monitors who comes in)
- [ ] 1st floor living area (general interior coverage)
- [ ] Basement / utility space (if applicable)

---

## Chime Distribution

6 chimes total for a 3-floor, 4,800 sq ft house. Recommended placement:

| Chime | Proposed Location | Notes |
|---|---|---|
| Chime V3 #1 | 1st floor — kitchen/family room | High-traffic area, heard during most waking hours |
| Chime V3 #2 | 1st floor — foyer / entry | Near doorbell, catches anyone at the door |
| Chime V3 #3 | 2nd floor — hallway | Covers bedrooms |
| Chime V3 #4 | 3rd floor — office | Top floor is furthest from doorbell |
| ~~Chime Pro~~ | ~~Wi-Fi extender location~~ | **Deprecated** — removed from account |
| ~~Chime v1~~ | ~~Master bedroom~~ | **Deprecated** — removed from account |

> **Ring schedule tip:** Set quiet hours per chime in the Ring app — V3 supports per-device schedules. Master bedroom chime can silence 10pm–7am while others stay active.

---

## Ring Protect Subscription

**Status: Ring Protect Pro purchased — 1-year subscription, activated 2026-05-21 (renews ~2027-05-21).**

- Video history for all Ring cameras ✓
- Ring Alarm professional monitoring — unused (no Ring Alarm hardware; Konnected is the alarm path)
- eero Secure — redundant (eero Plus already active, which is the superior tier)
- **Pricing note:** Ring Basic is $9.99/device — at 4-5 Ring devices that's $40–50/month. Pro at $20/month flat was the correct call.

> **Noonlight for Alarmo is still a separate decision** — Ring Pro's monitoring only covers Ring Alarm hardware, not Konnected/Alarmo. See `projects/alarm_system.md`.

---

## HA Integration

Ring integration is already active (`Ring — cameras + doorbell` in `configuration.yaml`). New devices auto-appear once registered in Ring account — no HA configuration change needed.

**Entities created per new camera:**
- `binary_sensor.[camera_name]_motion` — triggers automations
- `camera.[camera_name]` — MJPEG stream (cloud-proxied by default)

**Existing automation pattern:** `last_activity` triggers → notification. New cameras follow the same pattern automatically once entities appear.

**Chimes:** Not surfaced as HA entities — they work through Ring's own doorbell/chime pairing. No HA automation needed for basic doorbell chime function.

---

## Installation Order

1. **Doorbell first** — anchors the Ring ecosystem; transformer install happens at the same time
2. **Chimes** — plug-in, can be done any time; register and distribute
3. **Outdoor cameras** — measure/confirm mounting locations before buying brackets
4. **Indoor camera** — decide placement, plug in

---

## Open Questions

- [ ] Confirm which chimes are already registered in Ring account
- [ ] Identify camera coverage zones (where are the gaps in current Ring coverage?)
- [ ] Measure existing doorbell wire run — is it already wired or battery-only?
- [ ] Confirm solar exposure at proposed downspout mounting locations

*Last updated: 2026-05-21*
*Status: Planning — no hardware installed yet*
