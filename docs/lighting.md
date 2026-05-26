# Lighting — Inovelli Blue Series

## Overview

Whole-house Zigbee switch rollout using **Inovelli Blue Series** switches. All switches use ZHA (not Zigbee2MQTT). Same SKU (`VZW31-SN`) for both dimmer and on/off locations — mode is firmware-configured.

**Protocol note:** Inovelli Blue Series is Zigbee, not Thread/Matter. ZHA gives full access to LED notifications, multi-tap events, and parameter configuration via "Manage Zigbee Device."

---

## Hardware Reference

| Product | SKU | Use Case |
|---|---|---|
| Blue Series 2-1 Smart Switch | VZW31-SN | All dimmer and on/off locations (same SKU, mode configured in firmware) |
| Blue Series Fan+Light | VZW36 | Fan locations with light kit — wall switch |
| Fan+Light Canopy Module | VZM35-SN | Pairs with VZW36; installed inside fan housing |
| Blue Series Aux Switch | VZW05 | Secondary positions in 3-way / 4-way runs |

---

## Rollout Status

### Outdoor Batch — In Hand (Arrived 2026-05-26)

**10× VZW31-SN + 5× VZW05** — ready to install.

**Install order:**

1. **5 single-pole first** (no aux complexity — learn the ZHA pairing flow):

| Location | Switch type | Notes |
|---|---|---|
| Front Hanging Light | 1× VZW31-SN | Exterior, single-pole |
| Flood Lights | 1× VZW31-SN | Exterior, single-pole |
| Patio Door | 1× VZW31-SN | Exterior, single-pole |
| High Flood | 1× VZW31-SN | Exterior, single-pole |
| Personnel Door (exterior) | 1× VZW31-SN | Exterior, single-pole |

2. **2 three-way after** (once single-poles are done):

| Location | Smart switch | Aux switches |
|---|---|---|
| Outside Garage Lights | 1× VZW31-SN | 1× VZW05 |
| Garage Lights | 1× VZW31-SN | 1× VZW05 |

**Carry-forward:** 3× VZW31-SN + 3× VZW05 → first indoor batch.

---

### Indoor Batches — Planned

Order one batch at a time after the previous batch is installed.

| Batch | Locations | Hardware | Notes |
|---|---|---|---|
| **2 — Master Bedroom Can Lights** | Master bedroom cans | 1× VZW31-SN | Single-pole dimmer; uses 3 carry-forward switches |
| **3 — Exterior w/ Aux** | (complete — included in outdoor batch) | — | — |
| **4 — Office** | Can Lights, Corridor, Fan | 3× VZW31-SN, 1× VZW36, 2× VZW05, 1× VZM35-SN | All verified; add presence sensor |
| **5 — Master Bedroom** | Fan, Bathroom Hallway, Switched Outlet | 3× VZW31-SN, 1× VZW36, 1× VZM35-SN | Fan = double-gang VZW31-SN + VZW36 |
| **6 — Family Room** | Can Lights, Fan, Switched Outlet | 2× VZW31-SN, 1× VZW36, 1× VZM35-SN, 2× VZW05 | Confirm neutral at wall box + canopy |
| **7 — Kitchen + Laundry** | Kitchen cans, Butler's Kitchen, Kitchenette, Laundry | 4× VZW31-SN, 5× VZW05 | Kitchen cans has 3 aux — confirm layout first |
| **8 — Foyer + Dining** | Foyer Big Light, Foyer Can Lights, Dining Chandelier | 3× VZW31-SN, 2× VZW05 | All dimmers; high-visibility 1st floor |
| **9 — Lindsay's Office** | Can Lights 1, Can Lights 2, Switched Outlet | 3× VZW31-SN | No aux switches |
| **10 — Remaining** | Upstairs Hallway, 3rd Floor Stairs, Front Room | 4× VZW31-SN, 6× VZW05 | Upstairs Hallway has 4 aux (5-position run) |

---

## Full Location Inventory

| Room | Circuit | Smart switch | Aux | Verified |
|---|---|---|---|---|
| Office | Can Lights | 1× dimmer | 1 | ✓ |
| Office | Corridor | 1× dimmer | 1 | ✓ |
| Office | Fan | VZW31-SN + VZW36 | — | ✓ |
| Kitchen | Can Lights | 1× dimmer | 3 | ✓ |
| Kitchen | Butler's Kitchen | 1× dimmer | 1 | ✓ |
| Kitchen | Kitchenette | 1× dimmer | — | ✓ |
| Laundry Room | Can Lights | 1× dimmer | 1 | ✓ |
| Family Room | Can Lights | 1× dimmer | 1 | ✓ |
| Family Room | Fan | VZW36 + VZM35-SN canopy | — | Wiring TBD |
| Family Room | Switched Outlet | 1× on/off | 1 | ✓ |
| Master Bedroom | Can Lights | 1× dimmer | — | ✓ |
| Master Bedroom | Fan | VZW31-SN + VZW36 | — | ✓ |
| Master Bedroom | Bathroom Hallway | 1× dimmer | — | ✓ |
| Master Bedroom | Switched Outlet | 1× on/off | — | ✓ |
| Upstairs Hallway | — | 1× on/off | 4 | ✓ |
| Exterior | Front Hanging Light | 1× on/off | — | ✓ |
| Exterior | Flood Lights | 1× on/off | — | ✓ |
| Exterior | Outside Garage Lights | 1× on/off | 1 | ✓ |
| Exterior | Personnel Door | 1× on/off | — | ✓ |
| Exterior | Patio Door | 1× on/off | — | ✓ |
| Exterior | High Flood | 1× on/off | — | ✓ |
| Garage | Garage Lights | 1× on/off | 1 | ✓ |
| Lindsay's Office | Can Lights 1 | 1× dimmer | — | ✓ |
| Lindsay's Office | Can Lights 2 | 1× dimmer | — | ✓ |
| Lindsay's Office | Switched Outlet | 1× on/off | — | ✓ |
| 3rd Floor Stairs | Hallway Light | 1× on/off | 1 | ✓ |
| Foyer | Big Light | 1× dimmer | 1 | ✓ |
| Foyer | Can Lights | 1× dimmer | — | ✓ |
| Front Room | Switched Outlet | 1× on/off | — | ✓ |
| Dining Room | Chandelier | 1× dimmer | 1 | ✓ |

**Totals:** 29× VZW31-SN · 3× VZW36 · 3× VZM35-SN · 18× VZW05

---

## Per-Switch Install Checklist

1. Cut power at breaker
2. Install VZW31-SN (or VZW36) at smart switch location
3. Install VZW05 at all secondary locations in the same run
4. Power on — switch joins ZHA automatically (hold config button if needed)
5. Name entity in ZHA; assign to room
6. Configure parameters via ZHA "Manage Zigbee Device" (dimmer curve, min/max brightness, LED color)
7. Create HA automations as needed (presence, scene control, multi-tap)

---

## HA Integration Notes

- All switches expose: light entity, button events, LED notification entity
- Multi-tap events → HA events → use for scenes
- **LED bar** can act as status indicator (e.g. orange during TOU peak, blue for notifications)
- OTA firmware updates available through ZHA
- **Naming:** Set friendly names at pair time in ZHA — much easier than renaming after the fact

---

## Open Questions (Before Ordering Fan Batches)

- [ ] **Family Room Fan** — confirm neutral wire at wall switch box and hot+neutral at canopy before ordering Batch 6 VZW36 + VZM35-SN
- [ ] **Office + Master Bedroom fans** — do existing fans have an aftermarket canopy receiver (VZM35-SN replaces it), or pull-chain only (requires new wiring)?
- [ ] **Kitchen Can Lights** — 3 aux switches implies 4-way or two 3-way runs; confirm layout before ordering Batch 7
