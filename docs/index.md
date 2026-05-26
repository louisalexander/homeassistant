# Home Command

**Complete smart home manual — Home Assistant on HAOS**

---

## The House

3-floor, ~4,800 sq ft, built 2006. Home Assistant OS running on a local Mini S13 PC, remotely accessible via Nabu Casa cloud. All config is version-controlled in this repository.

---

## System at a Glance

| Layer | What's running |
|---|---|
| **Hub** | Home Assistant OS 2026.5 — local network + Nabu Casa remote |
| **Zigbee** | ZHA with ZBT-2 USB dongle (channel 25) — ~15 devices |
| **Z-Wave** | Z-Wave JS — smoke detectors (planned), smart locks (planned) |
| **Climate** | 2× Ecobee thermostats (2nd + 3rd floor active; 1st floor pending install) |
| **Cameras** | 2× Ring Indoor + 2× Ring Outdoor Solar (pending) + 1× Wyze v3 (office) |
| **Security** | Konnected Alarm Panel Pro (purchased, not yet installed) |
| **Lighting** | Inovelli Blue Series Zigbee rollout in progress — outdoor batch installed |
| **Energy** | EM16P dual-panel monitors (pending install) + plug-level monitoring |
| **Voice** | Alexa (primary) + Apple Home (secondary via HomeKit Bridge) |
| **Automations** | 27+ automations across leak detection, presence, TOU energy, security |

---

## Quick Navigation

<div class="grid cards" markdown>

-   :material-floor-plan: **[Architecture](architecture.md)**

    System topology, integrations, protocols, and how everything connects.

-   :material-devices: **[Hardware](hardware.md)**

    Complete inventory of every device — installed, pending, and on-hand.

-   :material-robot: **[Automations](automations.md)**

    All 27+ automations documented — triggers, conditions, actions.

-   :material-thermometer: **[Climate](climate.md)**

    Ecobee thermostats, Eco+ TOU pre-conditioning, dashboard cards.

-   :material-cctv: **[Cameras](cameras.md)**

    Ring perimeter layer + Wyze interior layer — streams, motion, dashboard.

-   :material-lightning-bolt: **[Lighting](lighting.md)**

    Inovelli Blue Series Zigbee switch rollout — phases, locations, status.

-   :material-shield-home: **[Security & Alarm](security.md)**

    Konnected panel, leak sensors, mock door automations, smoke detectors.

-   :material-meter-electric: **[Energy](energy.md)**

    EM16P monitors, TOU rate sensors, plug-level power tracking.

-   :material-router-network: **[Network](network.md)**

    Eero mesh, TP-Link switch, MoCA backbone, Zigbee coordinator placement.

-   :material-format-list-bulleted: **[Entity Reference](entities.md)**

    Quick-lookup table of all key entity IDs by domain.

</div>

---

## Project Status

See [`PLAN.md`](https://github.com/louisalexander/homeassistant/blob/main/PLAN.md) for the live execution plan. High-level:

| Project | Status |
|---|---|
| House Energy — EM16P | Active — **overdue install** |
| Climate — 1st floor Ecobee | Ready — hardware in hand |
| Cameras (Ring + Wyze) | In progress |
| Alarm System (Konnected) | Ready — hardware in hand |
| Inovelli Lighting — Outdoor | **In hand — ready to install** |
| Inovelli Lighting — Indoor | Ordered one batch at a time after outdoor done |
| Smoke Detectors | Ready to order |
| Smart Locks | On hold — clear backlog first |
| Wyze Cameras | Complete |
| Gas Fireplace | Deferred — seasonal |

---

!!! tip "Keeping this manual current"
    This documentation is auto-deployed from the `main` branch on every push.
    Whenever a project advances or config changes, the relevant page here is updated in the same commit.
