# Lighting

## Current State

Most of the house uses standard manual switches today. Smart switches are being installed in phases — starting with the outdoor/exterior locations.

---

## Outdoor Smart Switches — Active

The exterior lighting circuits now use **Inovelli Blue Series** Zigbee smart switches. These behave exactly like normal switches (press the paddle to toggle) but also respond to voice and app control.

| Location | What it controls |
|---|---|
| Front Hanging Light | Front entry |
| Flood Lights | Front facade flood lights |
| Patio Door | Back patio |
| High Flood | High-mount flood lights |
| Personnel Door | Exterior side door |
| Outside Garage Lights | Garage exterior lights |
| Garage Lights | Inside the garage |

### Controlling Outdoor Lights

- **Manual:** Press the switch paddle like normal
- **Alexa:** "Alexa, turn on the [name]"
- **Apple Home:** Available in the Home app
- **Dashboard:** Accessible from the HA dashboard

---

## What's Coming — Whole-House Rollout

Smart switches are being installed throughout the entire house in batches. After outdoor, the install order is:

1. Master Bedroom can lights
2. Office (can lights, corridor, fan)
3. Master Bedroom (fan, bathroom hallway, switched outlet)
4. Family Room (can lights, fan, switched outlet)
5. Kitchen + Laundry
6. Foyer + Dining Room
7. Lindsay's Office
8. Upstairs Hallway, 3rd Floor Stairs, Front Room

Each batch is ordered and installed before the next is ordered, so the rollout is steady but not rushed.

**Fan locations** (Office fan, Master Bedroom fan, Family Room fan) use a dedicated Inovelli fan+light switch that provides separate control of the fan speed and light — replacing the existing pull-chain or single-switch setup.

---

## Technical Reference — Full Location Inventory

??? note "Expand for complete switch inventory"

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
